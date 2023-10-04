---
title: '【第4-2章】promise'
date: 2019-04-05 16:22:15
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">


## 一、promise的三种状态 
fuifilling reject pending，在没有获取到reject或者fulfilling时一直都是pending，直到有reject或resolve返回
promise变为fufilling或reject之后，状态不可逆。
pending状态，不会触发then和catch，resolved会触发then，reject会触发catch

## 二、then(res,rej)方法
then(res,rej)方法的两个参数，第一个是resolve方法，第二个是reject方法，reject方法可以在后面用catch的方法调用
then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。
第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。
```javascript
// promise实现隔三秒打印i的值
let i = 1
function timeout(ms,wenzi) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, i++);
  });
}

timeout(3000)
.then((value)=>{console.log(value);return timeout(3000)}) // 隔三秒 1 return的返回值如果是一个promise对象，那么下一个then就会执行这个promise对象的then
.then((value)=>{console.log(value);return timeout(3000)}) // 隔三秒 2 同上
.then((value)=>{console.log(value);return timeout(3000)}) // 隔三秒 3 同上
.then((value)=>{console.log(value);return timeout(3000)}) // 隔三秒 4 同上
.then((value)=>{console.log(value);return 3000})// 隔三秒 5 return的是一个实际值，那么这个值就会被包装为promise.resolved(3000)而返回，也就是说3000作为下一个then的参数而使用
.then((value)=>{console.log(value)}) // 立刻打印 3000 由上一个return返回的3000作为该then的参数，3000被包装为promise.resolve(3000)
.catch((value)=>{
  console.log('catch'+value);
});
```
```javascript
// 图片加载
let url = 'https://pics6.baidu.com/feed/4b90f603738da9775728172eeb1762108618e304.jpeg?token=caf3a7ab76f26a0a52b930268b9ddc89'
function loadImg (url) {
    return new Promise((resolve,reject)=>{
        let img = document.createElement('img')
        img.onload = function() {
            resolve(img)
        }
        img.onerror = function() {
            let err = new Error('加载失败')
            reject(err)
        }
        img.src = url
    })
}

loadImg('https://pics6.baidu.com/feed/4b90f603738da9775728172eeb1762108618e304.jpeg?token=caf3a7ab76f26a0a52b930268b9ddc89')
.then((data)=>{ 
    console.log(data)// data是resolve传过去的img
    // <img src="https://pics6.baidu.com/feed/4b90f603738da9775728172eeb1762108618e304.jpeg?token=caf3a7ab76f26a0a52b930268b9ddc89">
    return loadImg(url)
}).then((data)=>{ 
    console.log(data)// data是上一个return loadImg(url)中resolve传过去的img标签
    // <img src="https://pics6.baidu.com/feed/4b90f603738da9775728172eeb1762108618e304.jpeg?token=caf3a7ab76f26a0a52b930268b9ddc89">
    return url
}).then((data)=>{ 
    console.log(data)// data是上一个return 的url
    // https://pics6.baidu.com/feed/4b90f603738da9775728172eeb1762108618e304.jpeg?token=caf3a7ab76f26a0a52b930268b9ddc89
    return url
}).catch((err)=>{
    console.log(err)
})
// then中return的如果是promise对象，则下一个then根据上一个then返回的promise来传参，return如果返回的是其他内容（如字符串），则将其他内容（如字符串）包装为promise（如promise.then(img)），将该内容作为then的参数
```

## 直接获取resolve状态或reject状态的方法：
```javascript
let promise = Promise.reject()
let promise = Promise.resolve()
// 这样子状态就直接是reject或resolve
```

## 三、then和catch的状态改变
1. then正常返回**resolve**，内部有报错（或throw err）则返回**rejected**。
2. catch正常返回**resolved**，内部有报错则返回**rejected**。
3. **resolve触发then回调，rejected后续触发catch回调**，注意resolve是**不会触发catch回调的**。
4. promise.resolve()和promise.reject()只要**内部没报错**，后续还是**触发then回调**。
5. 最后的状态靠最后返回的结果为定
```javascript
const p1 = Promise.resolve().then(()=>{
    return 100
})
console.log(p1) // resolve，触发后续then回调

const p2 = Promise.resolve().then(()=>{
    throw new Error('then error') // 只要内部有报错，就到reject，触发后续catch回调
})
console.log(p2) // reject

Promise.resolve().then(()=>{
    console.log(1) // 1
}).catch(()=>{
    console.log(2) 
}).then(()=>{
    console.log(3)
}) // 1 3

Promise.resolve().then(()=>{
    console.log(1) // 1
    throw new Error('error1')
}).catch(()=>{ // catch执行完只要内部没报错也是返回一个resolve状态的promise欧赔
    console.log(2)
}).then(()=>{
    console.log(3)
}) // 1 2 3

Promise.resolve().then(()=>{
    console.log(1) // 1
    throw new Error('error1')
}).catch(()=>{
    console.log(2) // catch执行完只要内部没报错也是返回一个resolve状态的promise欧赔，所以下面的catch不执
}).catch(()=>{
    console.log(3)
}) // 1 2
```
**第五点我要单独说一下：**
**最后的状态靠最后返回的结果为定**
在下面的代码中，promiseA本身是反悔了resolve，但由于它引入了另一个promiseB，而promiseB返回reject，所以最终执行结果会到catch方法中。
```javascript
function getPromise(a,b) {
    let promiseA = new Promise((resolve,reject)=>{
        if(a>=b) {
            resolve(promiseB)
        }
        else {
            reject('ERROR')
        }
    })
    return promiseA
}
let promiseB = Promise.reject()
getPromise(1,2).then((value)=>{
    console.log(value)
}).catch((err)=>{
    console.log(err)
})
```

## 四. resolve和fufilled
**promiseA如果resolved到了另一个promiseB中，那它不一定是pendding fulfilled reject,它的状态由promiseB执行完毕才决定。**
上面有代码示例。

>(1) nothing happened yet
(2) "locked in"to another promise
(3) fulfilled 
(4) rejeted
(1)(2) pedding (3)(4)settled (2)(3)(4)resolved (1)unresolved

##五. Promise.resolve() 和 Promise.reject() 
1. 如果Promise.resolve()有参数:
（1）. 参数为promise，原封不动的返回该promise
```javascript
function testPromise (a) {
    let promise = new Promise((resolve,reject)=>{
        if(a) {
            resolve(a)
        }
        else {
            reject('undefined val')
        }
    })
    return promise
}
let promiseR = Promise.resolve(testPromise('111'))
// 写法相当于 let promiseR = testPromise('111')
promiseR.then((value)=>{
    console.log(a) // 111
})
```

（2）参数为有then方法的对象，会立即执行then方法，并将执行结果封装为promise对象
```javascript
let thenable = {
    then: function(resolve, reject) {
      resolve(42);
    }
  };
  
  let p1 = Promise.resolve(thenable);
  p1.then(function (value) {
    console.log(value);  // 42，注意已经执行过promise方法
  });
```
（3）参数没有then方法，或根本就不是对象，则将值包装为promise对象并返回到then中，状态为resolved
```javascript
const p = Promise.resolve('Hello');

p.then(function (s) {
  console.log(s)
});
// Hello
```
## 六、promise定义时立即执行，而then相当于异步，所以执行完promise内的内容跳出去执行同步代码，再执行then
```
new Promise((resolve)=>{
    console.log(1)
    resolve()
}).then(()=>{
    console.log(2)

})
console.log(3) // 1 3 2
```





### 面试题：
以下代码的执行结果？
```javascript
async function async1() {
    console.log("async1 start")
    await async2()
    console.log("async1 end")
}
 
async function async2(){
    console.log("async2")
}
 
console.log("script start")
 
setTimeout(function(){
    console.log("setTimeout")
}, 0)
 
async1()
 
new Promise(function(resolve){
    console.log("promise1")
    resolve()
}).then(function(){
    console.log("promise2")
})
 
```
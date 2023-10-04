---
title: '【第4-3章】async await'
date: 2019-04-06 17:01:05
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">



## 一、generator的基本用法
```javascript
function* generator(a) {
    yield console.log(a);
    let b = 111+222;
    yield a+b;
    let d = yield 765+3
    console.log(`d=${d}`)
    return 1234567
}
let gen = generator(333)
gen.next() // value:333,done:false,
gen.next() // value:666,done:false
gen.next() // value:768,done:false
gen.next(123) // d = 123 value:1234567(return为1234567，若函数没有return返回值则返回值为undefined),done:true
//next()函数的参数当做上一个yield语句的返回值。
```
**1. for...of循环可以自动遍历generator函数运行时生成的iterator对象，且此时不需要调用next()方法。**
```javascript
function* foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
}

for(let v of foo()) {
    console.log(v)
}
// 1 2 3 4 5 
```

**3. g.throw(err) 在generator函数内部排除一个错误**
**4. g.return() 可以返回特定的值，并且终结generator函数。**
**5. yield\* gen() 若是在generator函数内部需要调用其他generator函数，需要手动遍历，或直接使用yield\*表达式。**
**6. 如果一个对象的属性是generator函数，则可以简写，generator的返回值是iterator遍历器**
```javascript
let obj = {
    fun:function() {

    },
    fun1: function* () {

    }
}
// 等同于
let obj = {
    fun () {

    },
    *fun1 () {

    }
}
```

## 二、async、await
**· async 可以用同步的方式写异步的代码，async返回的是`promise`对象，如果返回了普通变量则会被包装成promise**
**· await后面一般追加异步操作，相当于`promise.then()`，如`promise`对象、其他async函数**

#### 1. async/await和Promise的关系，为什么要使用async
 async/await是消灭异步回调的方法，有了async/await，就可以使用同步的写法去写异步的程序，不需要再用回调函数，写起来也比promise简便。
相当于promise.then promise.catch，但二者并不冲突
promise捕获错误只能用promise.catch的方法，没办法用try...catch，但async/await可以，写法上会规范许多
 ```javascript
async function fn1 () {
    let a = Promise.reject(100)
    // Uncaught (in promise) 100
    const res = await a 
// 不执行，await相当于promise.then，
// 所以上面代码变为Promise.reject(100).then(()=>{})无法处理上面的reject，直接报错
    console.log(res)  
}
fn1() // Uncaught (in promise) 100
```

```javascript
async function fn1 () {
    let a = Promise.reject(100)
    // Uncaught (in promise) 100
    try{
        const res = await a // 不执行，await相当于promise.then，所以无法处理上面的reject，直接报错
        console.log(res)  
    }
    catch(e) {
        console.log(`e=${e}`) // e=100
    }
}
fn1() // e=100
```

#### 2. async返回一个promise，可以用.then方法，await相当于promise.then(重点！)，但用法又有所不同
async函数内部return语句返回的值，***会成为then方法回调函数的参数。***
async自动执行、async函数返回一个 Promise 对象，可以使用then方法添加回调函数。
await 后面跟promise对象或一个一个async 函数，相当于promise.then()
async函数返回的  Promise 对象，**必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。**也就是说，**只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。**
```javascript
async function fn1 () {
    return 100 // 相当于 return Promise.resolve(100)
}
fn1 == 100 // false，async返回一个promise，所以写值相当于promise.resolve(100)
fn1().then((n)=>{ // async方法因为返回的是promise所以可以加then
    console.log(`n=${n}`) // 100
})
```
#### 3. 正常情况下，await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。async函数return的则是一个promise对象
```javascript
async function fn1 () {
    let a = await 100 // 正常情况下，await命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。
   // a.then()报错
   // await虽然跟着一个promise对象，但返回结果可不一定是promise。
    console.log(a) // 100
}
```
```javascript
// await如果赋值了，这个值返回的then中的参数
async function fna() {
    let a = await Promise.resolve(100) 
// 理解为Promise.resolve(100).then()中then的参数
// then中参数为100，所以a为100
}
```

### 4. await后面可以跟promise或async函数，如果跟的是一个普通函数，那加不加await都是一样的
```javascript
function timeout(ms) {
    return new Promise((resolve) => {
      console.log('resolve=',resolve)
      setTimeout(()=>{resolve(100)}, ms);
    });
  }
  
  async function asyncPrint(value, ms) {
    let a = await timeout(ms);
    console.log('a='+a)
    console.log(value);
  }  //a =100
```

###5. async内的catch相当于promise.catch()
```
async function fn1() {
    try {
        const p4 = Promise.reject('err1')
        const res = await p4
        console.log(res)
    }
    catch(err) {
        console.log(err)
    }
}
fn1() // err1
```
### 若await后面的promise状态为reject，则之后代码均不执行
```javascript
async function fn1() {
    const p4 = Promise.reject('err1')
    const res = await p4
    console.log(res)
}
fn1() // Uncaught (in promise) err1
// 解决方法： 加try catch捕获
```

### 5.异步的本质：遵循event-loop
async/await只是语法糖，从语法层面将代码变为同步的写法，但实质是异步的，依旧基于event-loop去执行
await 后面的内容全部都理解为放在回调函数中，所以他们都是异步操作，所以要放在同步操作的后面去执行（event-loop原理）
```javascript
async function fn1() {
    console.log(1)
    await console.log(2)

        await console.log(3)

            await setTimeout(()=>{console.log(4)},3000)
            console.log(5)

                await console.log(6)
                console.log(7)
}
fn1() 
console.log(8) // 1 2 8 3 5 6 7 4
// 先执行fn1() 
// 执行console.log(1)
// 执行console.log(2)
// 执行第一个 await ,await后面部分为回调（异步操作），所以函数执行完毕，退出执行同步代码
// 执行console.log(8)
// 同步代码执行完毕，执行回调函数，即第一个await后面部分
// 执行console.log(3)
// 执行第二个 await
// 后面的部分为第二个await的回调，执行回调
// 执行setTimeout(()=>{console.log(3)},3000) ，定时器三秒启动
// 执行第三个await ，同样后面部分是第三个await的回调
// 执行console.log(5)
// 后面和上面同理，执行console.log(6)
// 执行第四个await，后面内容一样是它的回调
// 执行console.log(7)
```

### 6.await后面加不加promise，很很很重要
不加promise相当于一个同步函数，后面的代码也是同步代码了。
```
 function timeout(ms) {
    return new Promise((resolve) => {
      setTimeout(resolve, ms);
    });
  }
  
  async function asyncPrint(value, ms) {
    await timeout(ms);
    console.log(value);
  }
  
  asyncPrint('hello world', 5000);
  console.log(1)
// 打1，5秒后hello world
// 若不加promise，会先启动定时器，立即打印1和value的值，不会等待定时器完毕再打印value
// 加上promise，会先启动定时器，然后立刻打印1（同步操作），定时器结束打印value！

// 声明 new Promise的时候,promise里面的函数会立马被执行，声明是同步
// 声明之后.then.catch的内容才会被放到micro task queue中
```
```javascript
function timeout(ms) {
    return setTimeout(()=>{}, ms);
  }
  
  async function asyncPrint(value, ms) {
    await timeout(ms);
    console.log(value);
  }
  asyncPrint('hello world', 5000);
  console.log(1)
// 立刻打1 hello world
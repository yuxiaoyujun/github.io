---
title: 【第4-1章】event-loop、宏任务和微任务
date: 2020-07-11 05:42:55
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

# 宏任务和微任务
微任务：promise async为微任务
宏任务：浏览器自己规定的一些api，比如settimeout、dom事件、ajax请求
**微任务的时机在宏任务之前。顺序是同步任务、微任务、宏任务**

以下代码执行顺序为3、2、1

```js
setTimeout(() => {
    console.log(1)
}, 0);
Promise.resolve(2).then((data)=>{
    console.log(data)
}) 
console.log(3) // 3 2 1
```
# js是如何执行的？
先执行同步，再执行异步
从前到后，一行一行执行
若某一行报错，则停止执行下面的代码
先把同步代码执行完，再执行异步

# event-loop与dom渲染
### event-loop：
![](https://upload-images.jianshu.io/upload_images/20892169-942c612d621fcdca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

微任务和宏任务是两个不同的队列，微任务在micro task queue，宏任务在callback queue。
### 总结event loop过程
同步代码一行一行向下执行
遇到宏任务，先记录下，等待时机（定时、网络请求）
微任务放入微任务队列（micro task queue）
宏任务时机到了，移动到宏任务队列（callback queue）
若call stack为空（即同步代码执行完）eventloop开始工作
先执行当前的微任务，一条一条推入栈中执行
如果当前有微任务，还是放入微任务队列
微任务执行完毕，尝试dom渲染（不一定渲染，dom有更改则渲染）
轮询查找宏任务队列callback quene，如有则移动到call stack执行
然后继续循环以上过程。
除了放置异步任务的事件，"任务队列"还可以放置定时事件。也就是settimeout和setinterval，所以他们只是类似异步，但不是异步函数

虽然dom事件不是异步，但也用event-loop
**注意dom事件（dom事件不是异步）像click是立即执行，click事件中的代码才是需要等待event-loop轮询执行的代码**
**promise也是一样，promise.then之后的才是异步回调。**
比如下面的例子
```js
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
})

console.log(2)
```
```js
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
  Promise.resolve().then(_ => {
    console.log('before timeout')
    setTimeout(()=>{5},)
  }).then(_ => {
    Promise.resolve().then(_ => {
      console.log('also before timeout')
    })
  })
})

console.log(2)
```
结果：
1
2
3
before timeout 
also before timeout
4
5
## 从event-loop解释，为什么微任务执行时机更早
微任务是es6语法规定的 宏任务是浏览器规定的
微任务存储于micro task queue，不存储在web apis中
同步代码执行完毕-->清空call stack-->执行微任务-->dom渲染-->执行宏任务

## for循环里面有1000次循环，前500次每次都是把data赋值为1，后500次每次都是把data赋值为2，问：dom一共渲染了几次？
DOM一次都没有渲染，因为for循环是一个js执行的持续过程。js和DOM渲染是共享一个线程，js执行时DOM不会渲染。
js 和 DOM 渲染是共享一个线程，js 执行时 DOM 不会渲染。除非其中有宏任务。
---
title: '【第4章】同步和异步'
date: 2019-04-06 13:20:03
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 单线程和异步
js是单线程语言，只能同时做一件事情，两段js不能同时执行，.原因就是避免DOM渲染的冲突（都修改DOM就冲突了）
浏览器和nodejs已经支持js启动进程 如 web server
js和dom渲染共用一个县城，所以可以操作dom 
##异步为了解决什么问题？
因为js是单线程，异步为了解决遇到等待时，等待过程中不可能像alert一样阻塞程序进行，因此所有的“等待情况”都需要异步。

## 异步应用场景：
定时任务：setTimeout、setInterval、
网络请求：Ajax请求、动态<img>加载、
事件绑定：图片加载如onload，不用等待onload完毕再去执行onload后面的代码

## 回调地狱callback hell
回调函数： 将一个函数作为参数传递给另个函数
回调地狱用promise来解决
```javascript
function c() {}
 function a(function b(){
     function c(function d(){
 
     })
}) // 代码看起来过于复杂混乱
```
再如请求太多的时候，回调函数套回调函数
```javascript
$.get(url1, (data1) => {
    console.log(data1)
    $.get(url2,(data2)=>{
        console.log(data2)
        $.get(url3, (data3)=>{
            console.log(data3)
        })
    })
}) 
```
如果用promise去写，就会简洁很多。
```javascript
const url1 = 'data1.json'
const url2 = 'data2.json'
const url3 = 'data3.json'
function getData (url) {
    return new Promise((resolve,reject)=>{
        $.ajax({
            url:'url',
            success(data) {
                resolve(data)
            },
            error(err){
                reject(err)
            }
        })
    })
}
getData((url1).then(data1=>{

}).then(data2=>{

}).then(data3=>{

}).catch(err=>{
    console.log(err)
})
```
## 常见的面试题：
#### 图片加载
```
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
```
```javascript
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
// then中return的如果是promise对象，则下一个then根据上一个then返回的promise来传参。
// return如果返回的是其他内容（如字符串），则将其他内容（如字符串）包装为promise
//（如promise.resolved(img)），将该内容作为then的参数
```
#### 单线程是什么
#### 前端使用异步的场景有哪些
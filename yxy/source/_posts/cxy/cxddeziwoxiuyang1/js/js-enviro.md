---
title: '【第10章】运行环境'
date: 2019-04-16 14:06:12
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">


## 一、从输入url到渲染出页面的整个过程
1. dns解析： 域名--->IP
2. 浏览器根据IP地址向服务器发起http请求
3. 服务器处理http请求，并返回给浏览器
4. 根据HTML生成Domtree，根据Css生成cssom，再整合dom树和cssom整合的rendertree
5. 根据render tree渲染页面
6. 遇到script标签则暂停渲染，优先加载并执行js代码，完成再继续，直至渲染完成。
## 二、window.onload 和DOMContentLoaded的区别
```js
    window.addEventListener('load',function() {
        // 网页全部加载完之后运行（包括图片、视频、iframe）
    }) 
```
```js
    window.addEventListener('DOMContentLoaded',function () {
        // 网页dom渲染完即可运行，此时图片、视频可能没有加载完
    })
```
```js
    // 示例：
    let img1 = document.getElementById('img')
    console.log(img1)
    img1.onload = function(){
        console.log('img load') // 第2打印
        console.log(img.complete) // true
        alert(1)
    }
    img1.src = "https://www.hellobi.com/images/avatar.png"
    
    addEventListener('load',function(){
        console.log('load') // 第3打印
    })
    addEventListener('DOMContentLoaded',function(){
        console.log('DOMContentLoaded') // 第1打印
    })

```
## 三、性能优化的方案
比如防抖、节流、懒加载，可以把面试官往这个方向引入
#### 1. 原则
（1）.多使用内存、缓存或其他方法
（2）减少CPU计算量，减少网络耗时（空间换时间）
#### 2. 方法
（1）让加载更快：压缩代码（webpack mode production）服务端可以做打包
（2）减少访问次数：合并代码（利用webpack、雪碧图）、ssr服务器端渲染，缓存
（3）使用更快的网络： cdn ，cdn 内容分发网络，根据区域去访问不同地点的服务器。。。这样就会更快些
（4）让渲染更快： 
    css放在head js放在最下面
    尽早开始执行js，用DOMContentLoaded触发事件
    懒加载，图片上滑加载更多，如下7
    对dom查询进行缓存，如下8
    避免频繁DOM操作，合并到一起插入DOM结构
    节流、防抖（体验性优化）
<hr>

下面几条是对上面四条的补充
（5）缓存：5.1静态资源加hash后缀，根据文件内容计算hash 5.2文件内容不变，则hash不变，则url不变 5.3 url和文件不变，则会自动触发http缓存机制，返回304
（6）ssr：服务器端渲染：将网页和数据一起加载，一起渲染 | 非ssr（前后端分离）：先加载网页，在加载数据，再渲染数据
（7）懒加载
```js
    <img id = "img1" src = "previeww.png" data-realsrc = "abc.png"/>
    <script>
      var img1 = document.getElementById('img1')
      img1.src = img1.getAttribute('data-realsrc')
    </script>
```
（8）缓存dom查询

#### 4.防抖debouce
在频繁输入时，监听输入事件会频繁触发，像google搜索时，如果每一次输入都触发请求，请求量会很大，影响性能了。
所以设置一个在输入停止一小段时长后，再触发请求。会大量的减少请求量。
```js
// 防抖
let timer = null
input1.addEventListener('keyup',function(e){
    if(timer) {
        clearTimeout(timer)
    }
    timer = setTimeout(()=>{
        console.log(this.value)
        clearTimeout(timer)
    },300)
})
```
```js
// 封装
let input = document.querySelector('.input')
function debounce(fun,time = 300) {
    let timer = null
    return function () {
        if(timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(function() {
            fun()
            clearTimeout(timer)
        },300)
    }
}

input.addEventListener('keyup',debounce(function(){

},300))
```
#### 5.节流
```js
let div = document.querySelector('.dragDemo')
let timer = null
div.addEventListener('drag',function(e){
    if(timer) {
        return
    }
    timer = setTimeout(() => {
        console.log(e.offsetX,e.offsetY)
        timer = null
        // 这里不能用cleartimeout，因为即使cleartimeout了，timer的值也不为null，而是停在了当前的计数不再递增
    }, 300);
})
```
封装这里用到了**闭包**，之前在作用域和闭包中说过：
**闭包是由函数以及声明该函数的词法环境组合而成的**
**闭包的变量不会得到释放**
**闭包会随着函数的创建而被同时创建**
所以下面的timer向外层查找到的是形成的闭包的词法环境中的timer
```js
// 封装
function throttle (fun,time = 100) {
    let timer = null
    return function() {
        if (timer) {
            return
        }
        timer = setTimeout(()=>{
            fun.apply(this,arguments)
            timer = null
        },time)
    }
}
document.querySelector('.dragDemo').addEventListener('drag',throttle(function(e) {
    console.log(e.offsetX,e.offsetY)
},1000))
```
#### 6.xss
（1）. 问题：常见的web前端攻击方式有哪些？
比如我写了一篇博客，博客里面有script标签，该代码执行获取用户cookie的操作，任何用户访问该博客后，就会执行该代码。被获取到cookie之后，将cookie发送到我的服务器，这样就会拿到用户的敏感信息
（2）. xss预防：
//      替换特殊字符（< --> &lt; >  --> &gt;）前端后端都要替换。
#### 7.xsrf
（1）概念：
用?带参数去访问服务器，得到服务器数据
（2）预防方法：
    使用post接口、增加验证：例如密码、短信验证码、指纹


    
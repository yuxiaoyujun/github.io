---
title: '【第7章】ajax'
date: 2019-04-11 12:13:35
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、手写一个ajax
```js
// ajax发送请求的简单写法
let data = {
    name: '小明',
    age: '12'
}
let url = './sendmsg'

let xml = new XMLHttpRequest()
xml.onreadystatechange = function(xhr) {
    if(xhr.readyState == 4) {
        // readyState：0-->未调用send方法 1-->已调用send()，正在发送请求 2-->send()发送完成，已经接收到全部响应内容，3-->正在解析响应内容
        if(xhr.state == 200) {
            console.log(xhr.responseText)
        }
    }
}
// 上面的写好之后，下面的每次发送就直接写
```
promise ajax的写法
```js
// promise ajax写法
function ajax(url,data){
    return new Promise((res,rej)=>{
        let xml = new XMLHttpRequest()
        xml.onreadystatechange = function(xhr){
            if(xhr.readyState == 4) {
                if(xhr.state == 200) {
                    res({url,data})
                } else {
                    rej()
                }
            } else {
                rej()
            }
        }
        xml.open('POST',url,true)
        xml.send(data)
    })
}
ajax(url,data).then(({url,data})=>{

})
```
#### 二、跨域
###### 1. 同源
协议、域名、端口必须一致，叫做同源
同源策略：ajax请求时，浏览器要求当前网页和服务器端必须同源
###### 2. img、link、script 可以无视同源策略，即可以跨域
由于img可以无视同源策略，所以可以用于使用第三方统计服务，如打点
link script 可使用cdn，cdn一般是外域服
script可以实现jsonp
###### 3. 如何用jsonp实现跨域？
上面说了**script可以实现jsonp。**<br>
现在假设我需要在http://localhost:8081下访问http://localhost:8082的文件，由于端口不一致所以跨域了。
（1）首先，需要服务器进行配合。因为服务器可以拼接任意字段返回给服务器，所以让服务器将数据包装为js函数返回。

假设服务器给前端的数据为http://localhost:8082/index.js，该文件的内容让后端包装为callback方法，参数为真实要传递的数据，如：
```js
callback({
    name: '小明',
    age: 12
})
```
（2）然后，前端使用script标签引入该index文件
```js
<script src = "http://localhost:8082/index.js"></script>
```
这样，在页面中就引入了一个名为callback的函数，因为是script全局引入，所以该方法位于window下。<br>
（3）再使用另一个script标签，处理数据：
```js
window.callback = function(data){
   // 这个data就是后端传入callback的参数，即
  {
      name: '小明',
      age: 12
  }
}
```
###### 4. cors实现跨域
CORS 是纯服务器端操作，服务器端可以设置header根据下面方法设置响应头字段
服务器端可以直接设置header，去允许某个地址的跨域访问。
![](https://upload-images.jianshu.io/upload_images/20892169-efbefcbd891c78b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 5. 常用的ajax插件
fetch() axios 


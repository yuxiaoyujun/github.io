---
title: '【第8章】localStorage、sessionStorage、cookie'
date: 2019-04-12 15:33:35
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

内容：localStorage sessionStorage cookie

## 一、cookie 
#### 1. 什么是cookie？
cookie用来server端和浏览器端通信，并不是用来做本地存储，只是因为原先html5不普及所以借用来本地存储
#### 2. 前端修改cookie的方式：
document.cookie 
后端也可以修改cookie。
#### 3. cookie的缺点：
（1）存储大小最大只有4k
（2）每次发送http请求，cookie都会被发送到服务端，会增加请求的数据量
（3）只能用document.cookie来修改，很麻烦。

## 二、.localStorage和sessionStorage
#### 1. 优点：
（1）localStorage和sessionStorage是html5专门为存储设计，最大存储5m
（2）api简单易用
（3）不会随着http请求被发送。

localStorage和sessionStorage可以在控制台application查看
#### 2. api:
getItem(key) 
setItem(key,value)

#### 3. localStorage和sessionStorage、cookie区别
(1) localStorage会永久存储，sessionStorage只存在于当前会话，如果浏览器关闭，该网站的sessionStorage会立即清除。
 (2) cookie每次都会随着浏览器请求发送到服务端，localStorage和sessionStorage只存在于本地
 (3) localStorage和sessionStorage的api简单，cookie只能通过document.cookie设置。
---
title: 【nodejs】基础总结（四）路由的概念
date: 2019-08-11 16:02:32
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 简介

+ 路由可以被看作是一种地址解析和分发机制，它告诉服务器如何对不同的地址或URL进行响应。就像城市交通规则和地图帮助你找到目的地一样。

+ 它是构建Web应用程序的关键部分，用于确保请求被正确处理和响应。

+ 在 Node.js 中，HTTP 模块本身并没有内置路由功能，路由是开发者根据需求自己实现的一部分。

## 路由的作用

- 确定如何响应不同的URL路径或地址。
- 将客户端请求导向正确的处理逻辑。
- 帮助服务器区分和处理不同类型的请求。

## 使用路由的步骤

1. 创建Node.js应用程序。
2. 引入所需的模块，包括HTTP模块和路由模块。
3. 设置路由规则，定义URL路径与处理函数之间的映射。
4. 创建处理函数，用于执行与路由关联的操作。
5. 启动HTTP服务器，监听特定端口，并在请求到达时根据路由规则调用相应的处理函数。

## 使用Express.js框架实现路由

- Express.js是一个流行的Node.js Web应用程序框架。
- 它提供了灵活且易于使用的路由功能，以及许多其他有用的功能，如中间件、视图引擎、静态文件服务、REST API开发等。
- Express.js可用于构建具有动态内容的Web应用程序和API。

```js
// 引入Express模块
const express = require('express');
const app = express();
const port = 3000;

// 定义路由规则
app.get('/', (req, res) => {
  res.send('欢迎访问首页！');
});

app.get('/about', (req, res) => {
  res.send('关于我们页面');
});

app.get('/contact', (req, res) => {
  res.send('联系我们页面');
});

// 启动HTTP服务器
app.listen(port, () => {
  console.log(`应用程序运行在 http://localhost:${port}`);
});
```

## 不使用Express.js框架实现路由

```js
const http = require('http');
const url = require('url');

// 创建一个HTTP服务器
const server = http.createServer((req, res) => {
  // 解析请求的URL
  const parsedUrl = url.parse(req.url, true);

  // 获取请求的路径
  const path = parsedUrl.pathname;

  // 根据不同的路径进行路由分发
  if (path === '/') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('欢迎访问首页！');
  } else if (path === '/about') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('关于我们页面');
  } else if (path === '/contact') {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('联系我们页面');
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('页面未找到');
  }
});

// 监听端口
const port = 3000;
server.listen(port, () => {
  console.log(`应用程序运行在 http://localhost:${port}`);
});
```


---
title: 【nodejs】Nodejs基础总结之事件（二）事件类型
date: 2019-08-03 15:13:45
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 一、种类

Node.js 的事件系统主要包括以下几种事件类型和相关的事件：

1. **EventEmitter（自定义事件）**：这是 Node.js 事件系统的核心，你可以使用它来创建自定义事件和事件监听器。你可以注册自定义事件并为其添加监听器，然后在适当的时候触发这些事件。这个事件类型是最常用的。

2. **HTTP 事件**：Node.js 中的 HTTP 模块也支持事件，例如 `request` 和 `response` 事件。你可以监听这些事件以处理 HTTP 请求和响应。

   ```js
   const http = require('http');
   
   const server = http.createServer((req, res) => {
     res.end('Hello, World!');
   });
   
   server.on('request', (req, res) => {
     console.log('收到请求！');
   });
   
   server.listen(3000);
   ```
   
3. **文件系统事件**：Node.js 的文件系统模块（`fs` 模块）支持一系列事件，如 `open`、`close`、`data`、`end` 等，用于处理文件操作和读取。

   ```js
   const fs = require('fs');
   
   const readStream = fs.createReadStream('file.txt');
   
   readStream.on('data', (chunk) => {
     console.log('读取数据块：', chunk);
   });
   
   readStream.on('end', () => {
     console.log('读取完成！');
   });
   ```
   
4. **流事件**：Node.js 中的流（`stream`）是一种用于处理数据的抽象接口，它们支持多种事件，如 `data`、`end`、`error` 等。流事件用于处理数据的输入和输出。

5. **定时器事件**：Node.js 中的 `setTimeout` 和 `setInterval` 方法用于创建定时器，它们会触发定时器事件，让你可以执行定时任务。

   ```js
   setTimeout(() => {
     console.log('定时任务执行！');
   }, 1000);
   ```
   
6. **网络事件**：Node.js 中的网络模块（例如 `net`、`dgram` 等）支持事件，用于处理网络连接、数据传输和套接字事件。

   ```js
   const net = require('net');
   
   const server = net.createServer((socket) => {
     console.log('客户端已连接！');
   });
   
   server.on('close', () => {
     console.log('服务器已关闭！');
   });
   ```

这些是 Node.js 中常见的事件类型，但你也可以创建自定义事件类型，以满足应用程序的需求。Node.js 的事件系统是构建异步和事件驱动应用程序的核心部分，它允许你有效地管理和处理各种异步操作和事件。

## 二、EventEmitter介绍

`EventEmitter` 是 Node.js 事件系统的核心，它提供了用于创建、注册和触发事件的方法。以下是 `EventEmitter` 的一些重要属性和方法：

1. **EventEmitter 类**：`EventEmitter` 是一个构造函数，用于创建事件对象的实例。通常，你需要先引入 `events` 模块，然后创建一个自定义事件类继承自 `EventEmitter`。

   ```js
   const EventEmitter = require('events');
   ```

2. **on(event, listener)**：或**addListener(event, listener)**。用于注册事件监听器。当特定事件（由 `event` 参数指定）触发时，与之关联的回调函数（`listener`）将被执行。

   ```js
   emitter.on('eventName', (arg1, arg2) => {
     // 回调函数逻辑
   });
   ```

3. **once(event, listener)**：与 `on` 类似，用于注册事件监听器，但它只会在事件第一次触发时执行回调函数，之后就会被移除。

   ```js
   emitter.once('eventName', (arg1, arg2) => {
     // 只会执行一次
   });
   ```

4. **emit(event[, arg1][, arg2][, ...])**：用于触发特定事件，可选择传递参数给事件监听器。它会执行与事件关联的所有回调函数，并将参数传递给这些回调函数。

   ```js
   emitter.emit('eventName', arg1, arg2);
   ```

5. **removeListener(event, listener)**：从事件的监听器数组中移除指定的事件监听器。

   ```js
   emitter.removeListener('eventName', listenerFunction);
   ```

6. **removeAllListeners([event])**：从事件的监听器数组中移除所有监听器。如果提供了 `event` 参数，只会移除指定事件的监听器。

   ```js
   emitter.removeAllListeners('eventName');
   ```

7. **listenerCount(event)**：返回指定事件的监听器数量。

   ```js
   const count = emitter.listenerCount('eventName');
   ```

8. **setMaxListeners(n)**：设置单个事件可以添加的最大监听器数量，默认情况下为 10。如果需要更多监听器，可以使用这个方法来调整最大限制。

   ```js
   emitter.setMaxListeners(20);
   ```

## 三、自定义事件

以下是一个简单的示例，演示如何创建和触发自定义事件：

```js
const EventEmitter = require('events');

// 创建一个自定义事件类，继承自 EventEmitter
class MyEmitter extends EventEmitter {}

// 创建一个 MyEmitter 实例
const myEmitter = new MyEmitter();

// 注册事件监听器，监听 "customEvent" 事件
myEmitter.on('customEvent', (arg1, arg2) => {
  console.log(`事件触发了，参数1：${arg1}，参数2：${arg2}`);
});

// 触发自定义事件，传递参数
myEmitter.emit('customEvent', 'Hello', 'World');
```

在这个示例中：

1. 我们首先引入了 `events` 模块并创建了一个自定义事件类 `MyEmitter`，该类继承自 `EventEmitter`。
2. 然后，我们创建了一个 `MyEmitter` 的实例 `myEmitter`。
3. 使用 `.on` 方法，我们注册了一个事件监听器，它监听名为 "customEvent" 的自定义事件。当 "customEvent" 事件被触发时，监听器会执行回调函数，并打印传递给回调函数的参数。
4. 最后，我们使用 `.emit` 方法来触发自定义事件 "customEvent"，并传递两个参数（'Hello' 和 'World'）给事件监听器。

当运行这段代码时，会输出如下内容：

```
事件触发了，参数1：Hello，参数2：World
```


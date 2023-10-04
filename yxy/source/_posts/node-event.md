---
title: 【nodejs】Nodejsejs基础总结之事件（一）事件循环
date: 2019-06-01 11:02:45
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 一、nodejs 事件循环

### 1. 基本概念

Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数。

### 1.1 宏任务与微任务及其优先级

### **宏任务中**：

**1. timers**：定时器，setTimeout setInterval

**2. I/O callback**：文件读写、网络读写、流的读写、控制台读写等出现的错误或后续操作。

**3. Idle,prepare**：闲置状态。

- "Idle" 阶段通常用于执行一些与性能无关的、非紧急的任务，以充分利用系统的空闲时间比如：**垃圾回收**、**日志记录**等。
- "Prepare" 用于在系统进入空闲状态时进行一些准备操作，以便在下一次事件循环迭代中更快地响应事件，比如**资源准备**、**数据预取**、**状态检查**、**数据库连接**等。
- 大多数情况下，开发者不太需要直接操作这两个阶段。

**4. Poll 阶段**：执行poll中的I/O队列。

具体来说，"poll" 阶段的工作流程如下：

+ **进入 "poll" 阶段**：当事件循环进入 "poll" 阶段时，它会检查事件队列中是否有待处理的事件。
+ **等待 I/O 事件**：在 `poll` 阶段，Node.js 会等待并监听来自操作系统的事件，这些事件包括文件可读、可写、网络套接字的可读、可写等等。这些事件可能已经被触发，但需要在 `poll` 阶段中等待。
+ **处理 I/O 事件**：`poll` 阶段的主要任务是处理这些 I/O 事件。当某个事件发生时，Node.js 将会执行相应的回调函数。这些回调函数通常是由之前注册的事件监听器定义的。
+ **等待其他任务**：如果在 `poll` 阶段没有 I/O 事件需要处理，事件循环会进入**非阻塞的等待**状态，等待其他事件的发生或者其他任务的执行。这样可以充分利用系统的资源，不会空闲浪费。
+ **检查定时器**：在等待事件的同时，事件循环还会检查是否有已到期的定时器任务，如果有，会进入 `timers` 阶段执行相应的回调函数。

**5. check 阶段**： 存储`setImmediate`回调。

需要注意的点如下：

+ **"check" 阶段的执行顺序**：当 "poll" 阶段完成后，事件循环会检查 "check" 阶段是否有待处理的回调函数。如果有，它会依次执行这些回调函数。
+ **setImmediate 和 process.nextTick**：通常，通过 `setImmediate` 注册的回调函数会在 "check" 阶段执行。而使用`process.nextTick` 的回调函数优先级比 `setImmediate` 高。
+ **事件处理**：某些事件（如网络套接字的数据可用事件）也可以在 "check" 阶段处理，但通常是在其他阶段触发的，然后在 "check" 阶段执行与之相关的回调函数（如`socket.on('data')`）。

**6. close callback 阶段**：关闭回调

工作流程如下：

+ **资源清理**："close callbacks" 阶段用于执行与资源清理相关的回调函数。例如，在关闭一个网络连接时，可以在 "close callbacks" 阶段执行回调函数，用于释放相关的资源和资源的最后清理。

+ **异步关闭操作**：某些资源的关闭可能是异步的，例如在关闭网络套接字时可能需要等待未完成的数据传输完成。因此，相关的关闭回调函数会在异步操作完成后执行。

+ **`socket.on('close')` 事件**：网络套接字的 `close` 事件通常与 "close callbacks" 阶段相关联。当网络套接字关闭时，可以注册一个回调函数，以便在 "close callbacks" 阶段执行清理操作。

+ **文件描述符的关闭**：类似地，文件操作中，文件描述符的关闭也可能与 "close callbacks" 阶段相关。当文件被关闭时，可以执行清理回调函数。



### 微任务中：

+ 包括promise、async/await、process.nextTick
+ process.nextTick的优先级最高，如果没必要的话，可以使用setImmediate取代process.nextTick
+ 微任务的执行时机比宏任务高

### 总结 nodejs 的 eventloop

1. 执行同步代码
2. 执行微任务
3. 按顺序执行宏任务
4. 在每个宏任务结束时，都会执行当前所有的微任务。

### 网上看到一个有意思的练习题

还是那句话噢new promise本身是同步的，只是then之后的才是异步。

```js
process.nextTick(function () {
	console.log("1");
});
process.nextTick(function () {
	console.log("2");
	setImmediate(function () {
		console.log("3");
	});
	process.nextTick(function () {
		console.log("4");
	});
});

setImmediate(function () {
	console.log("5");
	process.nextTick(function () {
		console.log("6");
	});
	setImmediate(function () {
		console.log("7");
	});
});

setTimeout((e) => {
	console.log(8);
	new Promise((resolve, reject) => {
		console.log(8 + "promise");
		resolve();
	}).then((e) => {
		console.log(8 + "promise+then");
	});
}, 0);

setTimeout((e) => {
	console.log(9);
}, 0);

setImmediate(function () {
	console.log("10");
	process.nextTick(function () {
		console.log("11");
	});
	process.nextTick(function () {
		console.log("12");
	});
	setImmediate(function () {
		console.log("13");
	});
});

new Promise((resolve, reject) => {
	console.log(15);
	resolve();
}).then((e) => {
	console.log(16);
});
console.log("14");

```

执行结果：

```
15
14
1
2
4
16
8
8promise
8promise+then
9
5
6
10
11
12
3
7
13
```


---
title: 【nodejs】基础总结（五）http概念
date: 2019-08-11 16:02:32
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 五、http 相关

### 1.  创建服务器 createServer

```js
// 导入http模块
const http = require("http");
// 创建服务器对象
const server = http.createServer((req, res) => {
	res.setHeader("Content-Type", "text/html;charset=utf-8");
	res.end("server启动！");
});

server.listen(8181, () => {
	console.log("server start!");
});

```

### 结果

![](/images/image-20230926185731966.png)

### 2. 提取 http 报文

在`createServer`中，用`req`和`res`获取报文。

```js
const http = require("http");
const server = http.createServer((req, res) => {
	// 每一个请求都会走一次下面的逻辑
	console.log(req.method); // 请求的方法 如：get
	console.log(req.url); // 请求路径 如：/blog/index
	console.log(req.httpVersion); // http协议版本号 如：1.1
	console.log(req.headers); // 请求的header
	res.setHeader("Content-Type", "text/html;charset=utf-8");
	res.end("server启动！");
});
server.listen(8182, () => {
	console.log("success!");
});

```

![](/images/image-20230927154206240.png)

### 3. 路径解析

在nodejs中，有url模块，可以用来解析路径。

之前在`createServer`时，已经在`req`中返回了`url`路径，然后可以使用nodejs内置的url模块进行路径的解析。

```js
url.parse(url, true); // 将一个地址字符串转换Url对象
```

示例代码如下：

```js
const http = require("http");
const url = require("url");
const server = http.createServer((req, res) => {
	const parseUrl = url.parse(req.url, true);
  // 传true时可以将解析的Url对象中的query变为对象。
	console.log(parseUrl);
	res.end("success!");
});

server.listen(8184, () => {
	console.log("已启动");
});
```

结果：

当访问`http://localhost:8184/?a=1&b=2`时，返回结果如下：

![](/images/image-20230927165936711.png)

Url.query数组中第一个元素返回Object: null prototype的意思是，该数组是Object，prototype指向null 

### 4. 路径解析的另一种方法

可以`new URL`实例。

如

```js
let url = new URL(req.url,'http://a.b.c')
```

这两个方法的区别是，`nodejs`内置的`url`模块可以解析一个没有域名的地址，而`new URL`创建的实例对象必须要是**一个完整的地址**（有域名）

但由于这里解析不关心域名是什么，只是为了获取当前路径的`search`，所以随便写一个假域名都得。

```js
const http = require("http");
const url = require("url");
const server = http.createServer((req, res) => {
	const url = new URL(req.url, "http://a.b.c");
	console.log(url);
	res.end("end");
});

server.listen(8184, () => {
	console.log("已启动");
});

```

![](/images/image-20230927172028583.png)

可以看到`searchParams`这个属性，用`searchParams`的`get`方法去获取参数

如：

```js
url.searchParams.get('a')
```

### 5. 响应报文（response相关）

+ ### 设置response状态码

```js
response.statusCode
```

如

```js
response.statusCode = 200
```

```js
response.statusCode = 402
```

+ ### 设置响应状态码的描述

```js
response.statusMessage
```

如

```js
if(response.statusCode == 501){
	response.statusMessage = '用户不符合要求'
}
```

+ ### 设置响应头

```js
response.setHeader('content-type','text/html;charset=utf-8')
```

当需要设置多个同名响应头时，可以使用

```js
response.setHeader('set-cookie',['aaa','bbb','ddd'])
```

+ 使用`setHeader`和`writeHead`

`setHeader` 和 `writeHead` 都是用于设置HTTP响应头的方法，它们有一些区别。

1. `setHeader`

    方法：

   - `response.setHeader(name, value)` 方法用于设置指定名称的响应头字段的值。如果已经存在具有相同名称的头字段，则将其值替换为提供的值。
   - 这个方法可以多次调用，每次调用都会设置或覆盖指定头字段的值。
   - 通常用于设置单个响应头字段的值。

示例：

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.setHeader('Content-Type', 'text/html');
  res.setHeader('Cache-Control', 'max-age=3600');
  res.setHeader('X-Frame-Options', 'DENY');

  res.end('<h1>Hello, World!</h1>');
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

1. writeHead

    方法：

   - `response.writeHead(statusCode, [reasonPhrase], [headers])` 方法用于同时设置响应的状态码、状态描述和一组响应头字段。
   - 这个方法通常在响应的开始部分被调用，用于一次性设置多个响应头字段和响应的状态信息。
   - 如果你使用了 `writeHead`，那么后续的 `setHeader` 调用将**覆盖之前设置的同名头字段的值**。

示例：

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html', 'Cache-Control': 'max-age=3600', 'X-Frame-Options': 'DENY' });
  
  res.end('<h1>Hello, World!</h1>');
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

区别：

`setHeader` 主要用于设置**单个响应头**字段的值

`writeHead` 用于**同时设置响应的状态码、状态描述和多个响应头字段的值**，通常在**响应的开始**部分使用。

选择使用哪个方法取决于你的需求和代码的结构。如果只需要设置一个或两个响应头字段，`setHeader` 可能更方便。如果需要设置多个响应头字段和状态信息，`writeHead` 可能更合适。

+ ### 设置响应体

1. `response.write`：
   - `response.write` 用于向响应流中写入数据，但不会结束响应。这意味着你可以多次使用 `response.write` 来逐步构建响应内容。
   - 当你使用 `response.write` 向响应流中写入数据后，响应还没有完成，客户端浏览器仍然等待进一步的数据。
   - 使用 `response.write` 可以用于分块传输数据，比如在处理大文件下载或流式响应时。
2. `response.end`：
   - `response.end` 用于结束响应并将其发送到客户端。一旦调用了 `response.end`，响应就会被完成，客户端将收到完整的响应内容。
   - 一般情况下，当你已经写入了所有需要发送的数据时，应该使用 `response.end` 来完成响应。
   - 调用 `response.end` 后，你不能再向响应流中写入更多数据，否则会引发错误。

+ #### 代码示例

```js
const http = require('http');

const server = http.createServer((req, res) => {
  res.setHeader('Content-Type', 'text/html');
  res.setHeader('Cache-Control', 'max-age=3600');
  res.setHeader('X-Frame-Options', 'DENY');

  res.end('<h1>Hello, World!</h1>');
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```



### 6. 请求报文

**`request`** 对象在请求处理函数中作为回调函数的第一个参数传递。它包含了有关客户端请求的信息，例如请求头、请求方法、请求 URL 等。

常见的属性及方法：

1. **request.url**:
   - 包含客户端请求的 URL 信息。
2. **request.method**:
   - 包含客户端请求的 HTTP 方法，如 GET、POST 等。
3. **request.headers**:
   - 包含客户端请求的所有 HTTP 头字段。

代码就不写了，其实上面处理url的部分已经写过了。




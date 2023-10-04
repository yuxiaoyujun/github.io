---
title: 【nodejs】path.join和path.resolve的区别
date: 2019-08-10 16:10:37
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---
## 一、 参数拼接 path.join([path1][, path2][, ...])

**`path.join()`方法可以连接任意多个路径字符串。要连接的多个路径可做为参数传入。**

path.join()方法的参数为string，可以加多个参数，最后会拼在一起形成一个地址，若不是string则报错
```js
// 用法
const path = require('path'); 
path.join('https://', 'www.', 'aaa', '.com', '/','aaa') 
'https://www.aaa.com/aaa' 

// 传入的不为字符串则报错
path.join('aaa',{},'bbb') 
// 抛出的异常 TypeError: Arguments to path.join must be strings'
```

## 二、 路径解析：path.resolve([from ...], to)

**path.resolve()方法可以将多个路径解析为一个规范化的绝对路径。**
其处理方式类似于对这些路径逐一进行cd操作，但resolve在未执行时不会校验其合法性（就是可以不存在这个地址）

```js
path.resolve('foo/bar', '/tmp/file/', '..', 'a/../truefile')
```
相当于
```bash
cd foo/bar
cd /tmp/file/
cd ..
cd a/../truefile
pwd
```
举例：
```js
path.resolve('/foo/bar', './baz') 
// 输出结果为 '/foo/bar/baz' 
path.resolve('/foo/bar', '/tmp/file/') 
// 输出结果为 '/tmp/file' 

path.resolve(__dirname, 'static_files/png/', '../gif/image.gif') 
// 当前的工作路径是 /home/itbilu/node，则输出结果为 
// '/home/itbilu/node/wwwroot/static_files/gif/image.gif'

```
**注意**：`resolve`的第一个参数如果是__dirname，之后出现了`/xxx`的路径，则会被解析为相对于根目录的绝对路径。拼接时需要注意这一点。

如：
``` js
const fs = require("fs");
const path = require("path");

console.log(path.resolve(__dirname, "/node")); // 根目录下
```
and
``` js
const fs = require("fs");
const path = require("path");

console.log(path.resolve(__dirname, "node"));  // __dirname所在文件夹下的相对路径下
```
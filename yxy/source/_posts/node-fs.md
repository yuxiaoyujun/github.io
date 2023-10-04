---
title: 【nodejs】基础总结（二）fs模块
date: 2019-08-08 17:20:37
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 三、 fs模块

### 1. 简介

fs模块可以实现创建文件、删除文件、修改/写入文件、读取文件、移动文件等，还可以操作文件夹。可以实现与硬盘有关的操作。



### 2. fs.writeFile() 写入文件（异步）

**语法**：`fs.writeFile(file,data,options?,callback(err)?)`

**参数**：

- File 文件名，文件若不存在，会被自动创建
- data 写入的数据
- options 可选的选项
- callback 回调，参数为err，若成功则err返回为null

**使用**：

```js
const fs = require("fs");
fs.writeFile("./test.txt", "随便写一段", function (err) {
	if (!err) {
		console.log("写入成功");
	}
});
console.log("先执行");
```

![](/images/image-20230829182854658.png)

**注意点**：

+ `fs.writeFile()`是一个异步方法。



### 3. fs.writeFileSync() 写入文件（同步）

**语法**：`fs.writeFileSync(file,data,options?)`

**参数**：

- File 文件名，文件若不存在，会被自动创建
- data 写入的数据
- options 可选的选项

**使用**：

```js
const fs = require("fs");
fs.writeFileSync("./test.txt", "随便写一段");
console.log("后执行");
```

**注意点**：

+ `fs.writeFileSync()`是一个同步方法，会逐行向下执行。

+ 它和`fs.writeFile()`不同的地方在于没有回调函数。



### 4. fs.appendFile() 追加文件内容

**语法**：`fs.writeFile(file,data,options?,callback(err)?`

**参数**：

- File 文件名，文件若不存在，不会被自动创建
- data 写入的数据
- options 可选的选项

**使用**：

```js
const fs = require("fs");
fs.appendFile("./test.txt", "\r\n多加一段");
```

![](/images/image-20230829191437742.png) 

**注意点**：

+ 参数与`fs.writeFile()` 一致

+ `fs.writeFile()`也可以实现追加内容。只需要将第三个参数的option追加

```js
const fs = require('fs')
fs.writeFile('./test.txt','\r\n多加一行',{flag:'a'})
```



### 5. fs.appendFileSync() 追加文件内容（同步）

略，参数与`fs.writeFileSync() `一致，

**注意点**：

+ 文件若不存在，不会被自动创建。



### 6. fs.createWriteStream()

**语法**：

+ `fs.createWriteStream(file)` 创建流
+ `fs.write(content: string)` 写入流
+ `fs.close()` 关闭流（可选）

**参数**：

- File 文件名，文件若不存在，会被自动创建

**使用**：

```js
const fs = require("fs");
const cws = fs.createWriteStream("./test.txt");
cws.write("aaa\r\n");
cws.write("bbb\r\n");
cws.write("ccc\r\n");

cws.close(); // 不写也没关系，因为在脚本执行完毕后，会自动关闭流
```

![](/images/image-20230830120353420.png)

**注意点**：

+ `fs.close()`可以不调用
+ 应用场景是，比如安装软件，安装软件的过程就是一个文件写入硬盘的过程。再比如写logs等等。

### 7. fs.readFile() 读取文件（异步）

**语法**：`fs.readFile(file,options?,callback(err,data)?)`

**参数**：

- File 文件名，文件若不存在，会被自动创建
- options 可选的选项
- callback 回调，参数为err，若成功则err返回为null

**使用**：

```js
const fs = require("fs");
fs.readFile("test.txt", function (err, data) {
	if (!err) {
		console.log(data.toString()); // data是buffer，需要将buffer转为字符串
	}
	return;
});
```

![](/images/image-20230830181214440.png)

**注意点**：

### 8. fs.readFileSync() 读取文件（同步）

略，基本一致，就是没有回调函数



### 9. fs.createReadStream() 流式读取

**语法**：`fs.createReadStream(file)`

**参数**：

- File 文件名，文件若不存在，会被自动创建
- options 可选的选项
- callback 回调，参数为err，若成功则err返回为null

**使用**：

```js
const fs = require("fs");
const crs = fs.createReadStream("./test.txt");

// 使用 on 触发 data 事件
// 'data' 的意思是 绑定 data事件。默认一次读取的一块为65536
crs.on("data", (chunk) => {
	console.log(chunk.length); // 查看一次读取的长度
	console.log(chunk.toString());
});

// 使用 end 方法结束读取事件（非必要）
crs.on("end", () => {
	console.log("完成");
});
```

**注意点**：

+ 流式读取，读取的是文件的一部分内容，普通的读取是一次性读取全部内容。一次读取长度默认为65536字节。
+ chunk.length可以查看读取的长度
+ 该方法一般用于读取大文件，它占用内存空间比直接用readFile要小。
+ 通过以下代码，可以看两种读取方式所占用内存的不同大小

```js
const fs = require("fs");
const process = require("process");
const crs = fs.createReadStream("./test.txt");

// 使用 on 触发 data 事件
// 'data' 的意思是 绑定 data事件。默认一次读取的一块为65536

crs.on("data", (chunk) => {
	console.log(chunk.length); // 查看一次读取的长度
	console.log(chunk.toString());
});

// 使用 end 方法结束读取事件（非必要）
crs.on("end", () => {
	console.log("完成");
	console.log(process.memoryUsage()); // 查看内存状况，rss就是所占用的内存空间，33095680
});

// const rd = fs.readFile("./test.txt", (err, data) => {
// 	if (!err) {
// 		console.log(data.toString());
// 		console.log(process.memoryUsage()); // 查看内存状况，rss就是所占用的内存空间，53690368
// 	}
// });
```



### 10. fs.rename(oldPath, newPath, callback) 重命名文件（异步）

**语法**：`fs.rename(oldPath: string,newPath: string,callback?)`

**参数**：

- oldPath 源文件路径，如./test.txt
- newPath 源文件路径的新名称，如./testRename.txt
- callback 回调，参数为err，若成功则err返回为null

**使用**：

```js
const fs = require("fs");
fs.rename("./test.txt", "./testRename.txt", (err) => {
	if (!err) {
		console.log("成功");
	}
});

```



### 11. fs.renameSync(oldPath, newPath) 重命名文件（同步）

略

### 12. 文件删除

在nodejs中，文件删除有两种方式

+ `fs.unlink(filename)` 与 `fs.unlinkSync(filename)`
+ ``fs.rm(filename)`与`fs.rmSync(filename)`

```js
const fs = require("fs");
fs.rm("./test.txt", (err) => {
	if (!err) {
		console.log("成功");
	}
});
```

注： `fs.rm`方法在14.4版本以上支持

### 13. mkdir(dir, option?,callback) 和mkdirSync(dir, option?,callback) 创建文件夹 

**语法**: 

+ fs.mkdir(path,option,callback)

**使用**：

```js
fs.mkdir("./test/test1/test.txt", { recursive: true }, (err) => {
	if (!err) {
		console.log("成功");
	}
});

```



### 14. 读取文件夹 readdir与readdirSync

**语法**: 

+ fs.mkdir(path,option,callback)

**使用**：

```js
fs.readdir("./test", (err, data) => {
	if (!err) console.log(data);
});
```



### 15. 删除文件夹 rmdir() 与rmdirSync()

**语法**: 

+ fs.mkdir(path,option,callback)

**使用**：

```js
fs.rm("./test", { recursive: true }, (err) => {
	if (!err) {
		console.log("删除成功");
	} else {
		console.log(err);
	}
});
```



**注意点**： 

1. 支持递归删除
1. 可能被废除，不建议使用
1. rm方法不仅可以用来删除文件。
1. `{ recursive: true }`的意思是级联删除

### 16. 查看文件状态 fs.stat(file,callback)

**语法**: 

+ fs.stat(file,callback)

**使用**：

```js
const fs = require("fs");
fs.stat("./", (err, data) => {
	if (!err) {
		console.log(data);
		console.log(data.isDirectory());
		console.log(data.isFile());
	} else {
		console.log("操作失败了！");
	}
});

```



**结果**：

![](/images/image-20230926154400250.png)

**注意点**：

1. 返回的data参数中，`isFile` 方法表示判断是否为文件。`isDirectory` 方法表示判断是否为文件夹。

2. 一般在用于制作资源管理器时，文件名可以使用`readdir`和`readfile`的方式读取，而具体信息则使用`stat`方式读取

![](/images/image-20230926154800545.png)

### 17.  __dirname 来表示绝对路径

表示`__dirname` 所在**文件夹**的绝对路径

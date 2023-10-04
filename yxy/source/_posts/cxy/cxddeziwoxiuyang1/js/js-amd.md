---
title: '模块化之AMD和CommonJS'
date: 2021-04-13 12:00:38
tags: js
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、AMD
### 概念：
异步加载模块的规范，即，加载模块不会阻塞之后代码的执行。
AMD是为浏览器环境所设计的，如requirejs。
### 用法：
#### 1. 定义模块
AMD的模块，通过define函数定义在闭包中，格式如下：

```javascript
define(id?: string,dependencies?: string[],factory: Function | object)
```

**`id`**: 模块名，可选，若不写，则模块为匿名模块。

**`dependencies`**: 所要依赖的模块列表，字符串数组类型，可选。若未指定`dependencies`，默认值为`["require", "exports", "module"]`。

```javascript
define(function(require, exports, module) {}）
```

**`factory`**: 模块的具体实现，类型为对象或函数。

#### 2. 引入模块

通过`require`引入
```javascript
require([
	"moduleA",
	"moduleB",
	function (m1, m2) {
		// 使用m1、m2
	}
]);
```

#### 例子：
下面通过`requirejs`写个例子：

##### 1. 建立如下的目录结构：
![image.png](https://upload-images.jianshu.io/upload_images/20892169-a1c46888e1da89da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2. 在index.html引入requirejs，并通过data-main标识入口文件
要注意入口文件是唯一的。
```javascript
<script
	src="./require.js"
	data-main="js/main.js"></script>
```
##### 3. 在moduleA、moduleB中定义模块
moduleA.js
```javascript
define(function () {
	function logStr(str) {
		console.log(str);
	}
	return { logStr };
// 必须要将需要导出的内容return，且被花括号包裹上
// 或define的第三个参数就是一个对象,require才能接收到
});

```
moduleB.js
```javascript
define(["component/moduleA"], function (moduleA) {
// function的参数moduleA和前面的模块一一对应，如果引入多个模块，那么function就写多个参数去接收，可以改名。
// 引入moduleA的方法如下：
	moduleA.logStr("moduleA被moduleB引入");
	function logNum(num) {
		console.log(num);
	}
	return { logNum };
});
```
main.js
```javascript
require(["component/moduleB"], function (moduleB) {
	console.log("ModuleB被main引入，打印数字");
	moduleB.logNum(2);
});
```
也可以使用require.config去定义公用路径：
```javascript
require.config({
	paths: {
		moduleB: "./component/moduleB"
	}
});
require(["moduleB"], function (moduleB) {
	console.log("ModuleB被main引入，打印数字");
	moduleB.logNum(2);
});

```

##### 运行结果：
![](https://upload-images.jianshu.io/upload_images/20892169-02f2299da74151fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 二、CommonJS
同步加载模块的规范，即，只有加载完成，才能执行后续操作。
CommonJS 是以在浏览器环境之外构建 JavaScript 生态系统为目标而产生的项目，如nodejs。

### 用法

```javascript
// moduleA.js
module.exports = function( value ){
    return value * 2;
}
```

```javascript
// moduleB.js
var multiplyBy2 = require('./moduleA');
var result = multiplyBy2(4);
```

CommonJS 是同步加载模块，但其实也有浏览器端的实现，其原理是将所有模块都定义好并通过 `id` 索引，这样就可以方便的在浏览器环境中解析了，可以参考 [require1k](https://github.com/Stuk/require1k) 和 [tiny-browser-require](https://github.com/ruanyf/tiny-browser-require) 的源码来理解其解析（resolve）的过程。

更多关于 CommonJS 规范的内容请查看 [http://wiki.commonjs.org/wiki/CommonJS](http://wiki.commonjs.org/wiki/CommonJS)。

CommonJS的写法比AMD要简单明了很多，不举例了。
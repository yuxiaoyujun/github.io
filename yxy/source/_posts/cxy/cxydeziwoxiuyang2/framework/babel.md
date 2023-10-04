---
title: 'babel配置'
date: 2021-10-25 06:21:49
tags: babel
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 一、使用
### 安装
```javascript
npm install --save-dev @babel/core @babel/cli @babel/preset-env 
```
### 配置文件

Babel 有两种并行的配置文件格式，可以一起使用，也可以独立使用。

+ #### 全项目配置
`babel.config.json`/`babel.config.js`文件
+ 注：可以是js文件，可以是json文件

Babel在项目根目录中自动搜索babel.config.json文件，或使用受支持扩展名的等效文件
+ #### 文件相关配置
`.babelrc.json`/`.babelrc.js`文件
+ 注：可以是js文件，可以是json文件
Babel通过从正在编译的“文件名”开始搜索目录结构来加载`.babelrc.json`文件，直至找到包含`package.json`的目录下。

## 二、预设
预设是指预先在babel内部设置好的插件，可以直接使用。

示例：
```javascript
{
	preset: [
		"@babel/preset-env",
		{
			target: {
				edge: 17,
				firefox: 60,
				safari: 111
			}
		},
		{ useBuiltIns: "usage" },
	]
}
```
### 1. preset
+ **@babel/preset-env 用于编译 ES2015+ 语法**
+ **@babel/preset-typescript for TypeScript 编译ts，替代了ts-loader**
+ **@babel/preset-react for React**
#### 使用preset预设
在配置文件中添加presets字段，**执行顺序是从右到左。**
```javascript
{
  "presets": ["@babel/preset-env"]
}
```

### 2. target
`targets` 是需要兼容的浏览器版本
+ 他的值有：` android, chrome, deno, edge, electron, firefox, ie, ios, node, opera, rhino, safari, samsung`
+ 可以设置成一个`string`，比如` > 0.5%`,` last 2 versions, not dead` 意思是有大于百分之零点五的人使用，前两个版本，并且没有废弃

### 3. useBuildIns
***useBuildIns需要安装corejs使用***
```javascript
npm i core-js -S
```
比如：
```javascript
{ useBuiltIns: "usage" },
```
+ `false`： 默认值，不做任何语法转换
+ `usage`：Babel 将检查你的所有代码，以便查找targets环境中缺失的功能，然后只把必须的 polyfill 包含进来
+ entry：引入所有的polyfill包，必须在入口文件加入` import "core-js/stable"` 才会生效

### 4. corejs
安装的corejs版本号

####什么是corejs?
babel编译只能针对浏览器行为，es6语法规定的，如`promise`、`async/await`则无法转义。这种情况下则需要配置corejs。
之前这个操作通过`babel-polyfill`插件，但在7.4之后该插件已被废弃。
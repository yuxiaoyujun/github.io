---
title: '模块化之ES6'
date: 2021-04-12 12:00:38
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
 
  <meta name="referrer" content="no-referrer">

## 一、export命令
### 1. 基本使用

```javascript
export var firstName = 'Michael';
export var lastName = 'kaol';
export var age = '10'
```
或

```javascript
var firstName = 'Michael';
var lastName = 'kaol';
var age = '10'
export {firstName,lastName,age};
```

### 2. export也可以输出函数和类

```javascript
export function fun() {
  return 'fun'
}  // 对外输出函数名为fun的函数
```

### 3. as关键字

```javascript
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
}
```
### 4. 下面这种写法是错的

```javascript
var a = 1
export a // 相当与export 1，而输出的应为接口，值是不能输出的
```
正确写法： 
```javascript
export var a = 1
```
```javascript
var a = 1
export { a }
```
```javascript
var a = 1
export { a as n }
```
## 二、import
import命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。

```javascript
import { firstName, lastName, year } from './profile.js';
```

## 三、export default
### 1. 基本用法

```javascript
export default function () {
  console.log('foo');
}
```

``export default`` 导出的一定是一个匿名的东西，即使接受的不是匿名函数或变量，因为从前面的例子可以看出，使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。但是，用户肯定希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。

```javascript
function foo() {
  console.log('foo');
}

export default foo; 
```

上面代码中，foo函数的函数名foo，在模块外部是无效的。加载的时候，视同匿名函数加载。
### 2. export default的正确写法

```javascript
export default 1 //正确
```

```javascript
let a = 1
export default a //正确
```

```javascript
export default var a = 1 // 报错！export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。
```

在export default时，import导出不用大括号，除非是解构赋值。

```javascript
let obj = {
  a: 1,
  b: 2,
  c: 3
}
export default obj
```
从其他文件导入时：

```javascript
import obj from 'xxx.js'
```

## 四、其他注意点
### 1. commonJS和import
虽然通过babel转码，commonJS的require和import可以写在一起，可是由于import是静态解析阶段执行，所以在require引入前，import会先于它执行，这样有可能会导致错误。
### 2. import的简易写法
当导出下面的文件时

```javascript
export let a = 1;
export let b = 2;
export let c = 3;
```

引入可以这样写

```javascript
import {a,b,c} from xxx
```

或

```javascript
import * from xxx
```

### 3. import 和 export 的复合写法
如果在一个模块之中，先输入后输出同一个模块，import语句可以与export语句写在一起。
```javascript
export { a, b } from 'xxx_module';
// 可以简单理解为
import { a, b } from 'my_module';
export { a, b };
```
## 五、import()
import()用于动态加载，它返回一个 Promise 对象。下面是一个例子。
```javascript
const main = document.querySelector('main');

import(`./section-modules/${someVariable}.js`)
  .then(module => {
    module.loadPageInto(main);
  })
  .catch(err => {
    main.textContent = err.message;
  });
```
### import可以用作
#### 1. 条件加载
```javascript
if(flag) {
  import('./xxx')
}
```
#### 2. 按需加载
```javascript
btn.addEventListener('click',function(){
  import('./xxx')
})
```
#### 3. 动态模块路径
```javascript
function fun(a) {
  let moduleFun = ''
  if(a=1) {moduleFun = '/blog/1'}
  if(a=2) {moduleFun = '/blog/2'}
  if(a=3) {moduleFun = '/blog/3'}
  return moduleFun
}
import(fun(1)) // 根据函数的返回结果加载不同模块
```


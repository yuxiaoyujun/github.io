---
title: 【面试相关】this的补充
date: 2019-05-11 06:55:39
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---

# 一、作用域与this
ES5只有全局作用域和函数作用域，没有块级作用域
作用域分为静态/词法作用域和动态作用域

### 0.声明提升和暂时性死区
(1).声明提升(var)
console.log(a); // undefined
var a = 1;
(2).暂时性死区(let)
console.log(a) // Uncaught ReferenceError: a is not defined
let a = 1;
注意：
在es5 strict mode，赋值给未声明的变量将报错。

### 1.静态作用域
静态作用域指的是一段代码，在它执行之前就已经确定了它的作用域，简单来说就是在执行之前就确定了它可以应用哪些地方的作用域(变量)
首先let和const声明的全局变量不再属于window
变量的作用域，除了this以外，全部遵循词法作用域的原则
即JS引擎总会从最近的一个域，向外层域查找；
### 2.动态作用域
动态作用域–函数的作用域是在函数调用的时候才决定的
在 JavaScript 中的仅存的应用动态作用域的地方：this 引用
动态作用域，作用域是基于调用栈的，而不是代码中的作用域嵌套；
作用域嵌套，有词法作用域一样的特性，查找变量时，总是寻找最近的作用域；
### 3.声明函数的方法
1.函数声明
```js
function a (a,b,c) {	return a+b+c;}
```
2.函数表达式
```js
var a = function (a,b,c) {	return a+b+c;}
```
3.Function构造函数
语法： new Function(参数1, 参数2, 参数3, 方法体)
```js
var a = new Function('a','b','c','return a+b+c')
```

### 4.变量进入作用域的方法
1. Language-defined：所有的作用域默认都会给出 this 和 arguments 两个变量名（global没有arguments
2. Function declarations（函数声明）：如 function foo() {}
3. Formal parameters（函数形参）：函数有形参，形参会添加到函数的作用域中;
4. Variable declarations（变量声明）：如 var foo，包括_函数表达式_
其实1324是文章写的，但我验证发现实际上是1234。
除了上下文顺序声明之外，若声明提升符合
函数声明和变量声明总是会被移动（即host）到它们所在的作用域的顶部。
而变量的解析顺序（优先级），与变量进入作用域的4种方式的顺序一致。
关于上下文：https://stackoverflow.com/questions/7493936/is-there-a-difference-between-the-terms-execution-context-and-scope
￼
### 5.this

每个作用域都会有this
在全局上下文（任何函数以外），this指向全局对象。
```js
console.log(this === window); // true
```
对象中的this指向该对象

在函数内部时，this由函数怎么调用来确定。

简单调用，即独立函数调用。由于this没有通过call来指定，且this必须指向对象，那么默认就指向全局对象。

严格模式下，this保持进入execution context时被设置的值。如果没有设置，那么默认是undefined。它可以被设置为任意值**（包括null/undefined/1等等基础值，不会被转换成对象）**。

在箭头函数中，this由词法/静态作用域设置（set lexically）。它被设置为包含它的execution context的this，并且不再被调用方式影响（call/apply/bind）。

当函数作为对象方法调用时，this指向该对象。

原型链上的方法根对象方法一样，作为对象方法调用时this指向该对象。
在构造函数（函数用new调用）中，this指向要被constructed的新对象。
Function.prototype上的call和apply可以指定函数运行时的this。
注意，当用call和apply而传进去作为this的不是对象时，将会调用内置的ToObject操作转换成对象。所以4将会装换成new Number(4)，而null/undefined由于无法转换成对象，全局对象将作为this。

ES5引进了`Function.prototype.bind`。`f.bind(someObject)`会创建新的函数（函数体和作用域与原函数一致），但`this`被永久绑定到`someObject`，不论你怎么调用。
它说创建新函数，可不是覆盖原函数，！！！！

this自动设置为触发事件的dom元素
### 6.JavaScript采用Lexical Scope。（静态范围作用于）
于是，我们仅仅通过查看代码（因为JavaScript采用Lexical Scope），就可以确定各个变量到底指代哪个值。

另外，变量的查找是从里往外的，直到最顶层（全局作用域），并且一旦找到，即停止向上查找。所以内层的变量可以shadow外层的同名变量。

### 7. Function vs. Block Scope
上面的内容有意无意似乎应该表明了，JS没有Block Scope。
除了`Global Scope`，只有function可以创建新作用域（Function Scope）。 不过这已经是老黄历了，ES6引入了Block Scope。

另外，with和try catch都可以创建Block Scope。

TODO:
https://github.com/creeperyang/blog/issues/16
作用域和this这就差不多了，晚上找些面试题试试水下
补：上下文和作用域
https://www.kancloud.cn/kancloud/javascript-prototype-closure/66359
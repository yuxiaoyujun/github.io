---
title: '【第3-1章】this'
date: 2019-04-05 12:00:00
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">


## 一、作用域与this
**ES5只有全局作用域和函数作用域，没有块级作用域**
**作用域分为静态/词法作用域和动态作用域**

#### 0. 声明提升和暂时性死区

**(1).声明提升(var)**
```javascript
console.log(a); // undefined
var a = 1;
```
**(2).暂时性死区(let)**
```javascript
console.log(a) // Uncaught ReferenceError: a is not defined
let a = 1;
```
注意：
在es5 strict mode，赋值给未声明的变量将报错。

#### 1.静态作用域
静态作用域指的是一段代码，在它**执行之前就已经确定了它的作用域**，简单来说就是在执行之前就确定了它可以应用哪些地方的作用域(变量)
首先let和const声明的全局变量不再属于window
变量的作用域，**除了this以外，全部遵循词法作用域的原则。**
即JS引擎总会从最近的一个域，向外层域查找；
#### 二、动态作用域
动态作用域–函数的作用域是在**函数调用的时候才决定的。**
在 JavaScript 中的**仅存**的应用动态作用域的地方：this 引用。
动态作用域，作用域是基于调用栈的，而不是代码中的作用域嵌套；
**作用域嵌套**：有词法作用域一样的特性，查找变量时，总是寻找最近的作用域；

#### 三、声明函数的方法
（1）.函数声明
function a (a,b,c) {	return a+b+c;}
（2）.函数表达式
var a = function (a,b,c) {	return a+b+c;}
（3）.Function构造函数
语法： new Function(参数1, 参数2, 参数3, 方法体)
var a = new Function('a','b','c','return a+b+c')

#### 四、变量进入作用域的方法
**1.Language-defined**：所有的作用域默认都会给出 this 和 arguments 两个变量名（global没有arguments
**2.Formal parameters（函数形参）**：函数有形参，形参会添加到函数的作用域中;
**3.Function declarations（函数声明）**：如 function foo() {}
**4.Variable declarations（变量声明）**：如 var foo，包括_函数表达式_

除了上下文顺序声明之外，若声明提升符合。
函数声明和变量声明**总是会被移动到它们所在的作用域的顶部**。
而变量的解析顺序（优先级），与变量进入作用域的4种方式的顺序一致。

关于上下文：https://stackoverflow.com/questions/7493936/is-there-a-difference-between-the-terms-execution-context-and-scope
```javascript
function testOrder(arg) {
    console.log(arg); // arg是形参，不会被重新定义
    console.log(a); // 因为函数声明比变量声明优先级高，所以这里a是函数
    var arg = 'hello'; // var arg;变量声明被忽略， arg = 'hello'被执行
    var a = 10; // var a;被忽视; a = 10被执行，a变成number
    function a() {
        console.log('fun');
    } // 被提升到作用域顶部
    console.log(a); // 输出10
    console.log(arg); // 输出hello
}; 
testOrder('hi');
/* 输出：
hi 
function a() {
        console.log('fun');
    }
10 
hello 
*/
```
#### 五、this
每个作用域都会有this。
在全局上下文（任何函数以外），this指向全局对象。
```javascript
console.log(this === window); // true
```
###### this在具体位置的具体指向：
1. 在函数内部时，this由函数怎么调用来确定。
简单调用，即独立函数调用。由于this没有通过call来指定，且this必须指向对象，那么默认就指向全局对象。
2. 严格模式下，this保持进入execution context时被设置的值。如果没有设置，那么默认是undefined。它可以被设置为任意值**（包括null/undefined/1等等基础值，不会被转换成对象）**。
3. 在箭头函数中，this由词法/静态作用域设置（set lexically）。它被设置为包含它的execution context的this，并且不再被调用方式影响（call/apply/bind）。
4. 当函数作为对象方法调用时，this指向该对象。
5. 原型链上的方法根对象方法一样，作为对象方法调用时this指向该对象。
6. 在构造函数（函数用new调用）中，this指向要被constructed的新对象。
7. Function.prototype上的call和apply可以指定函数运行时的this。
8. ES5引进了Function.prototype.bind。f.bind(someObject)会创建新的函数（函数体和作用域与原函数一致），但this被永久绑定到someObject，不论你怎么调用。
它说创建新函数，可不是覆盖原函数，！！！！
9. 闭包中的this（挺绕的，但无论下次看到理不理解，都要记住闭包套的闭包有可能返回window，需要分析有没有自执行函数）
为什么下面这段代码的结果是"The Window"？
```javascript
var name = "The Window";
var object = {
    name: "My object",
    getNameFunc: function() {
        return function() {
            return this.name;
        };
    }
}
object.getNameFunc()() // The Window
```
object.getNameFunc是对象调用的方法，里面的this是指向object，但object里面return的闭包，有自己的this和arguments。
 在《Javascript高级程序设计》中有写
>为什么匿名函数没有取得其包含作用域（外部作用域）的this对象呢？
每个函数被调用时，其活动对象都会自动取得两个特殊变量：this和arguments。内部函数在搜索这两个变量时，只会搜索到其活动对象为止，因此永远不可能直接访问外部函数中的这两个变量。

在[作用域和闭包](/2019/04/05/cxy/js/js-context/)说过，闭包的情况有函数作为返回值及函数作为参数被传递。同样的，还有像操作dom函数的闭包。
```javascript
function clickEvent () {
  console.log(this) // dragDemo
  return (function() {
    console.log(this) // window
  })() // 如果没有自执行，就需要具体情况具体分析了
}
document.querySelector('.dragDemo').addEventListener('click',clickEvent)
```
再深层一些，比如clickEvent这个函数返回的也是闭包，那么无论多少层，只要执行了，都是window。
```javascript
function clickEvent (fun) {
  return function() {
    fun()
  }
}
document.querySelector('.div').addEventListener('click',clickEvent(function(){
    console.log(this) // window
}))
```
**注意**：当用call和apply而传进去作为this的不是对象时，将会调用内置的ToObject操作转换成对象。所以4将会装换成new Number(4)，而null/undefined由于无法转换成对象，全局对象将作为this。

#### 六、JavaScript采用Lexical Scope。（静态范围作用域）
于是，我们仅仅通过查看代码（因为JavaScript采用静态范围作用域），就可以确定各个变量到底指代哪个值。
另外，变量的查找是从里往外的，直到最顶层（全局作用域），并且一旦找到，即停止向上查找。所以内层的变量可以覆盖外层的同名变量。
#### 七、Function vs. Block Scope
上面的内容有意无意似乎应该表明了，JS没有Block Scope。
除了Global Scope，只有function可以创建新作用域（Function Scope）。 不过这已经是老黄历了，ES6引入了Block Scope。
另外，with和try catch都可以创建Block Scope。

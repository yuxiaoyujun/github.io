---
title: '【第3-2章】作用域和闭包'
date: 2019-04-05 12:00:00
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、作用域和自由变量
#### 作用域:
作用域就是一个变量的合法使用范围。
作用域分为全局作用域、函数作用域及块级作用域（es6新增）。
#### 函数作用域
```javascript
function func () {
    let a = 1
}
```
#### 块级作用域（es6新增）
```javascript
if (true) {
    let a = 1
}
console.log(a) //报错 let声明的块级作用域
```
```javascript
if (true) {
    var a = 1
}
console.log(a) // 1 全局作用域
```

#### 自由变量：
一个变量在当前作用域没有定义，（注意是当前作用域，哪怕上级作用域定义了只要当前作用域没定义就不行）但被使用了，沿上级作用域一级一级找，若到全局作用域还没有找到，就报错。
## 二、闭包
·闭包就是作用域应用的一个特殊情况，闭包让**开发者可以从内部函数访问外部函数**的作用域，有两种情况：
（1）函数作为参数被传递 
（2）函数作为返回值
·自由变量的查找是在函数定义的地方！（重点，函数定义的地方是指函数体在哪里），向上级作用域查找
**注意：闭包中的变量会常驻在内存，得不到释放，因为判断不了闭包内的变量未来是否会用到**
**闭包是由函数以及声明该函数的词法环境组合而成的。**
#### （1）函数作为参数被传递 
```javascript
const a = 100
function print(fn) {
    const a = 200
    fn()
}
function fn() {
    console.log(a)
}
print(fn) // 100 fn定义是在全局作用域，所以a == 100
```
#### （2）函数作为返回值
```javascript
function create() {
    const a = 100
    return function() {
        console.log(a)
    }
}
const a = 200 
const fn = create()
fn() // 100
```
#### （3）自执行函数
```javascript
let a = (function(i){console.log(i)})(1) // 1
a // undefined，此处的自执行函数没有设定返回值，所以a为undefined

```
```javascript
var data = [];
for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}
```
![返回值全为3](https://upload-images.jianshu.io/upload_images/20892169-3d19c3d683e09a2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**function 里可没传参数，看好了是data后面有参数，其实就算传了也没什么影响，因为i的值已经在调用时决定是3了，虽然函数定义时决定了变量所在的作用域，但调用时才能决定变量在该作用域实际的值。**

### **重要！！！变量在定义时就确定了上级作用域，但需要在调用时才能确定变量定义时所在的上级作用域或当前作用域该变量的值，比如这里**

**i在调用时的值为3，即使外部定义了i = 10，最终查找还是在定义时的当前作用域查找i，当前作用域的i为3，所以输出全为3**

**改进方法**：形成自己的封闭的块级作用域。
**改进方法1**：for内的var变为let，因为let具有块级作用域，它形成了三个私有作用域相互不会干扰。
```javascript
for (let i = 0; i < 3; i++) {
    data[i] = function () {
      console.log(i);
    }
  }
```
**改进方法2**：自执行函数和闭包
匿名函数定义也是在匿名函数声明的地方，这里即return后，有名字的函数定义就在有名字的函数那里，然后一层一层向上查找
```javascript
for(var i = 0;i < 3; i ++) {
    (function(j){
        data[j] = function() {console.log(j)}
    })(i)
}
```
**自由变量的查找是在函数定义的地方，向上级作用域查找 ，而不是在执行的地方。**

## 三、this
详见3-1
this取值是在**函数执行**的时候决定的，而不是**函数定义**的时候决定的。

箭头函数的this取值是**取他的上级作用域的值**（静态作用域），普通function的this取值是在**函数执行**时确认的。

**分为以下几种情况：**
（1）作为对象方法被执行，this指向该对象。
（2）class的this表示它当前的实例，在构造函数（函数用new调用）中，this指向要被constructed的新对象。
（3）普通函数的this，在调用时才知道this的作用域。
（4）箭头函数的this固定指向上级作用域 
（5）call apply
函数内的this和函数在什么环境中定义没有关系，而只和自己的主体有关。

####call、apply、bind的区别和使用：
作用是在一个对象中调用另一个对象的方法和属性。

call、apply、bind的第一个参数均为需要调用属性或方法的对象，区别是apply的第二个参数为数组，而bind、apply参数不放入数组而是直接列出。

bing会返回一个函数，执行函数后才改变this，而call、apply会立即执行。
```javascript
let fullname = {
    getFullName (city,country) {
        return this.firstname+this.lastname+'from'+country+city
    }
}
let name = {
    firstname: 'bai',
    lastname:'xue'
}
```
###### bind的执行：
```javascript
let fullnamestring = fullname.getFullName.bind(name,'安康','陕西')
fullnamestring() //'baixuefrom陕西安康'
```
###### call的执行：
```
let fullnamestring = fullname.getFullName.call(name,'安康','陕西') //'baixuefrom陕西安康'
```
###### apply的执行：
```
let fullnamestring = fullname.getFullName.apply(name,['安康','陕西']) //'baixuefrom陕西安康'
```
#### 面试题：
###### 手写bind、call、apply
###### 闭包的实际使用场景
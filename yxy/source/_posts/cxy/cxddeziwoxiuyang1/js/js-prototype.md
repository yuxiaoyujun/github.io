---
title: '【第2章】原型和原型链'
date: 2019-04-04 14:13:15
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、原型的简单描述
#### 在一个类中：
实例的隐式原型，指向类的显式原型
类的方法定义于prototype中。
**son.\_\_proto\_\_ = Son.prototype**
![](https://upload-images.jianshu.io/upload_images/20892169-c3761b8085632913.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 在继承关系中：
子类的显式原型的隐式原型指向父类的显式原型，查找时就顺着这个链去查找，类的最顶端为object
子类也有隐式原型，指向的是父类。
**Son.prototype.\_\_proto\_\_ = Parent.prototype**
**Son.\_\_proto\_\_ = Parent**
![](https://upload-images.jianshu.io/upload_images/20892169-2a872db5f400936f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### call、apply、bind的区别和使用：
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
```javascript
let fullnamestring = fullname.getFullName.call(name,'安康','陕西') //'baixuefrom陕西安康'
```
###### apply的执行：
```javascript
let fullnamestring = fullname.getFullName.apply(name,['安康','陕西']) //'baixuefrom陕西安康'
```

class说白了还是语法糖，如果不用class，用原型链当然也可以定义继承关系。
比如我现在有一个类A，有一个子类B
```javascript
function A() {
  this.a = 'a' // 自身属性，不存在于原型链上
}
function B() {
  
}
A.prototype.aa = 'valueaa'
// 继承的实现
B.__proto__ = A
B.prototype.__proto__ = A.prototype

let b = new B()
b.aa //valueaa
b.a // undefined，
```
### 补充：in和hasOwnproperty的区别
两个方法都是判断对象中是否存在某个key，但hasOwnproperty不判断原型链上的属性和方法。
所以，依据上面的例子：
'aa' in b // truejavascript
b.hasOwnProperty('aa') // false
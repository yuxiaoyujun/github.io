---
title: 'class总结'
date: 2020-07-21 13:03:15
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、基本写法
类的方法、属性、静态方法、静态属性、私有方法、私有属性
继承、接口（没有）、抽象（没有）、final（这个有）

```javascript
class Student {
    constructor(name,age) {
        this._name = name
        this._age = age
    }

    toString() {
        return `(姓名：${this._name},年龄${this._age})`
    }
}

let s1 = new Student('小明','12')
s1.y // 12
```
#### 构造函数constuctor
```javascript
class Student {
    constructor(name,age) {
        this._name = name
        this._age = age
    }
    toString() {
        return `(姓名：${this._name},年龄${this._age})`
    }
}
```
#### 实例属性
可以使用this.xxx写在constructor方法中，也可以写在类的顶端。
```javascript
class Student {
    _age = 12;
    constructor(name) {
        this._name = name
    }
}
let s1 = new Student('小欧')
s1._name //小欧
s1._age // 12
```
实例属性不存在于原型链上，而是存在于单个实例中。如果constructor写了赋值，在创建实例时却没有赋值，那就会变成undefined拉
```javascript
class Student {
    _age = 12;
    constructor(name,age) {
        this._name = name
        this._age = age
    }
}
let s1 = new Student('小欧')
s1._name //小欧
s1._age // undefined
```
原型属性（使用prototype赋值）存在于原型链，用“类.prototype.属性名”可以调用到。
```javascript
Student.prototype.sex = '女'
s1.sex // 女
s2.sex // 女
```
类的方法是默认定义在原型链上，供所有实例共享的。但在类内部定义的变量是属于单个实例的，不存在与原型链上。
如果想在类外通过赋值的方法，定义属于原型链的属性，当然所有实例也都可以共享，但一旦修改所有实例的该属性都会修改，其实没有什么意义还容易出问题。所以虽然可以在原型链上定义实例，但一般来说没必要。除非真的确定这个属性在所有的实例上都是相同的。

#### getter、setter方法
在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。
get/set方法定义的变量不需要提前定义。
set方法必须要传入value
```javascript
class JuniorStudent {
    _name = '';
    _age = 0;
    _sex = ''
    constructor (sex) {
        this._sex = sex
    }
    get say () {
        return 'hi!'
    }
    set say (value) {
        console.log('say is'+value)
    }
}

let js = new JuniorStudent()
js.say = '111'
```
#### name
在类内部，可以使用类名.name去调用，获取类的名字
```javascript
class Me {
    getClassName () {
      return Me.name // 类名.name
    }
}
let m = new Me ()
m.getClassName () // Me
```
#### Class表达式
类可以用表达式的形式定义，在类外部调用时只能用表达式调用，如下面的例子，外部只能使用MyClass调用，但MyClass的实例还是属于Me
```javascript
let MyClass = class Me {
    getClassName() {
        return Me.name
    }
}
let mc = new MyClass() //不能用new Me()
mc.getClassName () //Me
```
#### 属性表达式
类的属性名可以用表达式表示
```javascript
let methodName = 'getString'
let MyClass = class Me {
    [methodName]() {
        return 'getString'
    }
}
let mc = new MyClass()
mc.getString () //getString
```
####静态属性
**静态属性定义在类上，不在实例上，所以只能用类名.属性名调用。**
定义静态属性的方法是在class外部，用类名.属性定义该属性
```javascript
// 旧写法
class Foo {
}
Foo.prop = 1;
Foo.prop // 1
```
es6的规定，class内只有静态方法，没有静态属性，但现在有个提案是用**static**在内部定义静态属性，所以也可以如下定义
```javascript
// 新写法
class Foo {
    static fooString1 = 'string1'
}
let foo = new Foo()
foo.fooString1 // undefined
Foo.fooString1 // string1
```
**静态方法的定义：**
```javascript
class Foo {
    static pringString = function (str) {
        return str
    }
}
Foo.pringString('111') //111
```
私有方法和私有属性
使用#定义私有属性。只能在类的内部使用(this.#count)，无法继承，无法在外部调用。
**暂时没有提供私有方法。**
```javascript
class MyClass {
    #name = 'aaa'
    constructor () {
        this.name = this.#name
    }
}
let mc = new MyClass()
mc.name // aaa
```
####静态块
有时候在类内部有一部分代码，是不用每次创建实例时都去调用的，比如静态属性的赋值。
所以es2022规定了静态块，在类生成时只执行一次，主要用于对静态属性的赋值，之后新建实例就不再运行了。
```javascript
class C {
  static x = ...;
  static y;
  static z;

  static {
    try {
      const obj = doSomethingWith(this.x);
      this.y = obj.y; // 或C.y
      this.z = obj.z;   // 或C.z 
    }
    catch {
      this.y = ...;// 或C.y
      this.z = ...;// 或C.z
    }
  }
}
```
#### new.target
该属性一般用在构造函数之中，返回new命令作用于的那个构造函数。如果构造函数不是通过new命令，new.target会返回undefined
```javascript

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```
##二、类的继承
```javascript
class Person {
  //...  
}
class Student extends Person {
  //...  
}
```
#### super方法与super关键字
###### 1. super方法
super方法**可以且只能**在子类构造函数中调用，用来调用父类的constructor()，**在子类的构造函数中必须要调用一次super()**
```javascript
class Person {
    constructor (x,y) {
        this.x = x
        this.y = y
    }
}
class Student extends Person {
    constructor (x1,y1,newstring) {
        super(x1,y1)
        this.newstring = newstring
    }
}
let s1 = new Student(1,2,'str')
s1.x // 1
s1.newstring // str
```

###### 2. super关键字
super作为对象时，在普通方法中，指向父类的原型对象（Point.prototype）；在静态方法中，指向父类。
由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。
因为类中定义的方法本就是定义在原型链(prototype)上，所以super可以调用到类内的方法。但super没办法调用到类内的变量，因为类中定义的变量是存在于实例的。
```javascript
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m // undefined
```
在子类普通方法中通过super调用父类的方法时，方法内部的this**指向当前的子类实例**。
```javascript
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```
在子类的静态方法中通过super调用父类的方法时，方法内部的this指向当前的子类，而不是子类的实例。
```javascript
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```
---
title: 'typescript总结笔记'
date: 2020-09-10 15:20:18
tags: 
  - typescript
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">

## 什么是typescript？
typescpt是一种***静态类型语言***。
添加了类型的js，完全兼容js，写完后可以编译为js
我觉得是用另一种更为严格和规范的方式去写js
##静态类型语言
静态类型语言中，变量的类型必须先声明，即在创建的那一刻就已经确定好变量的类型，而后的使用中，你只能将这一指定类型的数据赋值给变量。如果强行将其他不相干类型的数据赋值给它，就会引发错误。
在静态语言中，一旦声明一个变量是int类型，之后就只能将int类型的数据赋值给它，否则就会引发错误，而动态类型则没有这样的限制，你将什么类型的数据赋值给变量，这个变量就是什么类型
## 强类型 VS 弱类型
强弱之分，体现在对类型的检查严格程度上，弱类型语言对于变量类型的检查比较宽松，容忍隐式类型转换这种事情的发生。何为隐式类型转换，一般有两种形式：

1. 相关类型之间隐式转换
2. 不相关类型之隐式间转换
## 一、基础类型
### 1. ts有哪些基础类型？
布尔、数字、字符串、数组、元祖、枚举、any、void、null、undefined、never、Object
### 2. 定义数组：
```javascript
let arr: Array<number> = [1,2,3]
let arr2: number[] = [1,2,3]
```
### 3. 定义元组：
```javascript
let x: [string,number,string] = ['aaa',111,'aaa']
```
注意：访问越界元素会报错
```javascript
x[3] = "world"; // Error, Property '3' does not exist on type '[string, number]'.
console.log(x[5].toString()); // Error, Property '5' does not exist on type '[string, number]'.

```
### 4. 枚举：定义类型
```javascript
enum Color {
    Red = '#ff0000',
    Green = '#00ff00',
    Blue = '#0000ff'
}
let color: Color = Color.red
```
### 5. never
never是任何类型的子类型，可以复制给任何类型，但没有类型是never的子类型或可以赋值给never类型（除了never本身），即使是any也不能复制给never。
never经常用于**抛出异常的函数**，或是**死循环**。

变量也可以是never类型，当他永不为真的类型保护所约束时。
```javascript
let n: never
let num: number = 1 // 报错
n = num // 报错
num = n // ok

// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}
// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}
// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```
### 6. void
void表示没有任何类型。和any完全相反。函数没有返回值时可以写void
```javascript
function warnUser(): void {
    console.log("This is my warning message");
}
```
void变量没有什么作用，因为void变量只能赋值null或undefined。
### 7. 类型断言
我可能有时候比ts更了解这个变量是什么类型，这时就可以用类型断言
语法有两种
第一种：**尖括号**
```javascript
let someValue: any = "this is a string";
let strLength: number =(<string>someValue).length;
```
第二种：**as**
```javascript
let someValue: any = "this is a string";
let strLength = (someValue as number).length
```
## 二、变量声明
ts用const、let代替了var
## 三、接口
### 1. 对象类型
定义一个类型，让变量按照这个类型来具体实现
```javascript
interface LabelType {
  label: string,
  name?: string, // name可选
  id: string|number,
  readonly size: number, // 只读
  [propName: string]: any, // 表示接口还可以有其他的属性，且属性名字任意，类型any
  fn(arg1: string): string
}
```

```javascript
let labelObj: LabelType {
    label: '标签1',
    size: 10,
    aaa: 32482093840, // [propName: string]: any;
    bbb: 'whatever',   // [propName: string]: any;
    fn(a) {
      return 'a'
    }
}
```
### 2. 函数类型
```javascript
interface Square{
  (source: string,subString: string):boolean
}
```
上面代码意思是，一个函数类型叫Square，它应该有两个参数，一个source类型string，一个subString类型string，返回boolean类型的值。
```javascript
let sq: Square = function(source: string,subString: string) {
  return true
}
```
### 3. 类类型
```javascript
interface ClockInterface {
  currentTime: Date
}
class Clock implements ClockInterface {
  currentTime: Date = new Date()
}
```
接口亦可以继承
```javascript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
## 四、类
```javascript
// 1. 基本使用class Person
// 2. 继承
// 3. 公共、私有、保护
// 4. 抽象类和接口的区别
```
```javascript
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

class Rhino extends Animal {
    constructor() { super("Rhino"); }
}

class Employee {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

let animal = new Animal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
animal = employee; // 错误: Animal 与 Employee 不兼容.
```
### 1. public、protected、private的区别：
protected：可以继承，不可以在类外部使用
private：不可以继承，不可以在类外部使用
public：可以继承，可以在类外部使用
### 2. readonly：
```javascript
class Person {
  readonly name: string;
  readonly sex: string = 'F'
  constructor (name: string) {
    this.name = name
  }
}
let p1 = new Person('小鸣')
```
### 3. getter、setter
```javascript
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```
### 4. 静态属性、静态方法
使用static定义，只能用类名.方法/属性使用
### 5. 抽象类与抽象方法
```javascript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```
### 6. 抽象类和接口的区别
#### （1）abstract类 
abstract类是定义用来被继承的类。
抽象类中**可以有抽象方法，可以有普通方法**。非抽象类也可以。
但抽象类**不能创建对象**，普通类继承抽象类**必须实现其方法，或将其定义为抽象方法**。
抽象类必须在类前用abstract关键字修饰。
**因为抽象类中含有无具体实现的方法，所以不能用抽象类创建对象。**

##### 1）抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。

##### 2）抽象类不能用来创建对象；

##### 3）如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法。如果子类没有实现父类的抽象方法，则必须将子类也定义为为abstract类。

##### 4）非抽象类中可以有抽象方法，比如继承的时候，如果子类不想实现父类的抽象方法，则必须将该方法也定义为抽象方法

#### (2) interface接口： 是对类的补充
接口中可以含有 变量和方法。但是要注意，接口中的变量会被隐式地指定为```public static final```变量（并且只能是```public static final```变量，用```private```修饰会报编译错误）

而方法会被隐式地指定为public abstract方法且只能是public abstract方法（用其他关键字，比如private、protected、static、 final等修饰会报编译错误），并且接口中所有的方法不能有具体的实现

**抽象方法是一种特殊的方法：它只有声明，而没有具体的实现。**
## 五、函数
```javascript
function add(a: number,b:number): number {
  return a + b
}
let addNum: number = add(1,2)
```
### 六、泛型（尖括号）
#### 1. 泛型变量
一个函数在公用的时候，若同一个参数可能有多个类型or可能有多种返回值的情况下，如果写成any会不严谨
```javascript
function identify(arg: any) : any {
   return arg // 如果我想让arg和函数返回值是一个类型，这种写法并不能保证是一个类型
}
```
用下面这个就可以保证返回值与参数相同类型
```javascript
function identity<T,Y>(arg: T,arg2: Y): T {
    return arg; // 这里T作为新类型可以保证返回值与参数是一个类型
}
// 使用：
identity<number,string>(1,'aaa')
identity<string,number>('aaa',1)
```
#### 2. 泛型类、泛型接口
```javascript
class GenericNumber<T> {
  public zeroValue: T
  public add(num1:T,num2:T): T {
    return num1
  }
}
let g = new GenericNumber().zeroValue
```
```javascript
interface Person<TT,YY> {
  name?: TT,
  sex?: TT,
  age?: YY,
  [propName:string]: any,
  identity?() : YY
}
let pe: Person<string,number> = {
  name: '111',
  age: 111,
  sex: 'nv',
  aaa: 1
}
```
## 七、枚举
```javascript
  enum Color {
    RED = 1,
    GREEN = 2,
    BLUE = 3
  }
  console.log(`Color.RED=${Color.RED}`)
  let red:Color = Color.RED
```
## 八、高级类型
### 1. 类型别名
```javascript
type NameOrId = string | number
let n1: NameOrId = 1
let n2: NameOrId = 'n1'
```
### 2. 交叉类型
也就是用’&‘
```javascript
interface Person {
  name: string,
  age: number
}
interface Pet {
  petName: string,
  petAge: number
}
type PersonAndPet = Person & Pet
let p1: PersonAndPet = {
  name: '小鸣',
  age: '11',
  petName: '喵',
  petAge: '1'
}

```
### 3. 联合类型
也就是用'|'
let arg: number | string = 'sfsdfds'
###4. 类型保护和类型断言
typeof和instanceof
#### （1）typeof
```javascript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
let n: NameOrResolver = '111'
if (typeof n === 'string') {
        return n;
    }
    else {
        return n();
    }
```
#### （2）instanceof只对类有用。
```javascript
class A {

}
let a: A = new A()
a instanceof A // true
```
## 九、Symbols
## 十、迭代器
for of 与for in
## 十一、模块（原：外部模块）
 “内部模块”现在称做“命名空间”。 “外部模块”现在则简称为“模块”，这是为了与 [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/)里的术语保持一致，(也就是说 `module X {` 相当于现在推荐的写法 `namespace X {`)。
```javascript
import {ModuleA,ModuleB,ModuleC} from './module'
```
用法和js一致。

## 十二、命名空间（原：内部模块）
 “内部模块”现在称做“命名空间”。 “外部模块”现在则简称为“模块”，这是为了与 [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/)里的术语保持一致，(也就是说 `module X {` 相当于现在推荐的写法 `namespace X {`)。
### 1. 命名空间的基本用法
在一个文件中，有时候有些变量是不想定义为全局的，比如有ab两个类被c类引用，如果都定义为全局的变量，不好维护容易出错，可以只对外暴露出c类接口（export class C）给该文件其他部分使用。。
```javascript
namespace Home{
    class A {
    
    }
    class B {
    
    }
    export class C {
        constructor () {
            new A()
            new B()
        }
    }
}
let abc = new Home.C()
// let abc1 = new A() // 报错
```
### 2. 复用
将逻辑与业务分离，可以将一个namespace用另一个namespace引入
如下面，用page引入components的命名空间
components.ts文件：
```javascript
namespace Components {
    export class C {

    }
    export class D {

    }
}
```
page.ts文件：
```javascript
namespace Home{
    export class C {
        constructor () {
            new Components.C()
            new Components.D()
        }
    }
}
let abc = new Home.C()
```
由于这种写法很难搞懂命名空间是在哪个文件中，所以最好在path.ts头部中用三斜线指定进行指定
```javascript
/// <reference path="components.ts" />
```
###3. 子命名空间
```javascript
namespace Components {
    // ...
    export namespace E {
        export class F {}
        class G {}
    }
}
```
## 十三、三斜线指令
三斜线指令***仅可放在包含它的文件的最顶端。***放在其他地方则会被当做***注释***去解析的。
包括下面几个指令，去看[官网](https://www.tslang.cn/docs/handbook/triple-slash-directives.html)吧
```javascript
/// <reference path="..." />
// 在编译过程中要引入的额外的文件。
```
```javascript
/// <reference types="..." />
// 用来声明依赖，被/// <reference path="..." />引用
```
```javascript
/// <reference no-default-lib="true"/>
```
```javascript
/// <amd-module />
```

tsc -w 监听，自动编译
tsc init 生成tsconfig.
tsconfig的配置：
outFile
outDir
module
rootDir
target

## 十四、声明文件
使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。
说人话：就是第三方库定义的一些东西直接引进来ts识别不了，ts只能识别js有的和ts已经定义的东西。所以需要另外写一份声明文件告诉ts，然后ts就知道这个第三方库有这些变量就不会报错了。
文件叫*.d.ts

declare 就是用在声明文件中的。
*   [全局变量](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#global-variables)
*   [全局函数](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#global-functions)
*   [带属性的对象](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#objects-with-properties)
*   [函数重载](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#overloaded-functions)
*   [可重用类型（接口）](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#reusable-types-interfaces)
*   [可重用类型（类型别名）](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#reusable-types-type-aliases)
*   [组织类型](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#organizing-types)
*   [类](https://www.tslang.cn/docs/handbook/declaration-files/by-example.html#classes)

## 十五、项目配置
tsconfig.json

 
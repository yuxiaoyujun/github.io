---
title: '【第1章】变量类型和计算'
date: 2019-04-03 13:03:15
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">


## 一、值类型和引用类型
#### 1.js的类型一共六种
数字（number）、字符串（string）、布尔值（boolean）、undefined、null、对象（Object）。 

**其中对象类型包括**：数组（Array）、函数（Function）

**还有两个特殊的对象**：正则（RegExp）和日期（Date）

**值类型**: number、string、boolean、undefined

**引用类型**: null、object(array function regexp date)

**新类型**：BigInt 是一种内置对象，它提供了一种方法来表示大于 253 - 1 的整数。

**BigInt和Number的区别**：不能用于 Math 对象中的方法；不能和任何 Number 实例混合运算，两者必须转换成同一种类型。在两种类型来回转换时要小心，因为 BigInt 变量在转换成 Number 变量时可能会丢失精度。

#### 2. 深入分析值类型和引用类型在内存中的存储方式
**值类型和引用类型有什么区别**
值类型在栈中存储，引用类型在堆中存储，值类型赋值多少就是多少，引用类型会被指针影响。
```javascript
let a = {a:1} 
let b = a
b.a // 1

b.a = 2 
a.a // 2
```
#### 3. typeof能判断那些类型？
typeof返回结果为字符串

```javascript
let a = 1
let b = '1'
let c
let d = {}
let e = Symbol('a')
typeof a // number
typeof b // string
typeof c // undefined
typeof d // object
typeof Array // function 构造函数
typeof Object // function 构造函数
typeof Symbol // symbol
// 六种数据类型和两种特殊类型，object内包括function和array，
// const不能定义引用类型
// typeof能判断的数据类型只有number string undefined object function，无法判断null，array，regexp，date，这几个均为object
```

#### 4. 何时使用===何时使用==？
除了 == null之外，全部用 === 

先说 "==="，这个比较简单。下面的规则用来判断两个值是否===相等：

1、如果类型不同，就不相等

2、如果两个都是数值，并且是同一个值，那么[相等]；(！例外)的是，如果其中至少一个是NaN，那么[不相等]。（判断一个值是否是NaN，只能用isNaN()来判断）

3、如果两个都是字符串，每个位置的字符都一样，那么相等；否则不相等 。

4、如果两个值都是true，或者都是false，那么相等。

5、如果两个值都引用同一个对象或函数，那么相等；否则不相等。

6、如果两个值都是null，或者都是undefined，那么相等。

再说 "=="

如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较：

a、如果一个是null、一个是undefined，那么相等。

b、如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。

c、如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。像-1既不等于false也不等于true，2既不等于false也不等于true
2 > true, -1 < false 都是成立的

d、如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。 js核心内置类，会尝试valueOf先于toString;

    toString() 将变量转换为字符串
    valueOf() 取变量的真实值

```javascript
    let str = "abc"
    let num = 123
    let obj1 = {a:1}
    let obj2 = {a:1}
```
e、任何其他组合，都不相等。
```javascript
    // 几个最好记下来的点：
    NaN == NaN //false
    undefined == null //true
    undefined === null // false
    100 == '100' //true
    0 == '' // true
    0 == false // true
    false == '' // true
    '' == null // false
    '' == undefined // false

    let a = {}
    b = a
    a === b

    let fun = function() {}
    fun1 = fun
    fun === fun1
```
#### 5. 手写深拷贝:递归,
什么是深拷贝:浅拷贝如果遇到的是一个引用类型，那么拷贝的实际上是变量的地址，更改的时候会更改拷贝的原变量，深拷贝则会生成一个新地址
方法1：
```javascript
JSON.parse(JSON.stringify());
```
方法2：
```javascript
function clone(target) {
    if(typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {}
        for(const key in target) {
            cloneTarget[key] = clone(target[key])
        }
        return cloneTarget
    } else {
        return target
    }
}
```
改良
```javascript
function clone(target, map = new Map()) {
    if (typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {};
        if (map.get(target)) {
            // map可用对象做键值，存储键值为实际值，value为虚设，如果遇到子节点和父节点是一个值，则直接将父节点值返回。
            return map.get(target);
        }
        map.set(target, cloneTarget);
        for (const key in target) {
            cloneTarget[key] = clone(target[key], map);
        }
        return cloneTarget;
    } else {
        return target;
    }
};
```
#### 手写isEquals()函数
```javascript
// 5.2 手写isEquals()函数
function isObject(obj) {
  return typeof obj === "object" && obj !== null
}
function isEqual(obj1,obj2){
    if(!isObject(obj1) || !isObject(obj2)){
        return obj1 === obj2
    }
    if(obj1 === obj2) {
        return true
    }
    const obj1Keys = Object.keys(obj1).length
    const obj2Keys = Object.keys(obj2).length
    if (obj1Keys !== obj2Keys) {
        return false
    }
    for (let i in obj1) {
        const res = isEqual(obj1[i],obj2[i])
        if(!res) {
            return false
        }
    }
    return true
} 
```
#### 6. truly变量和falsely变量
两步非运算为true，则为truly变量，两步非运算为false，则为falsely变量，即
```javascript
!!a === true !!a === false
```
以下为falsely变量，其他都为truly变量
```javascript
!!0 === false
!!NaN === false
!!'' === false
!!null === false
!!undefined === false
!!false === false
```
if判断时的truly变量和falsely变量，若为falsely变量直接无法走入if循环

#### 强制类型转换和隐式类型转换
(1) 强制类型转换parseInt()和parseFloat()
```javascript
parseInt('1a') // 1
parseInt('a1') // NaN
parseFloat('1.5a') // 1.5
parseFloat('a1.5') // NaN
```
(2) 隐式类型转换 if 逻辑运算 == +、*拼接字符串
//字符串和数字相加会转为字符串，字符串和数字相乘/除/减会转为数字
```javascript
'1' * 1 // 1
'1' + 1 // 11
'1' + '2' // '12'
'1' + 2 + '3' // '123'
1 + '2' // 12
'2' - 1 // 1
2 - '1' // 1
```

#### 补：
hasOwnProperty:对象方法，返回一个boolean，判断是否拥有此对象，**且不是继承来的对象，也就是沿着原型链上查找到的。**

**map weakmap set weakset 区别:**
1.weakset值只能是对象
2.weakset是弱引用，即如果其他对象都不引用该对象，那么垃圾回收机制会立即回收。
weakmap键只能是对象，值可以任意。WeakMap的键名所指向的对象，不计入垃圾回收机制。WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。
set方法：has() add() delete() clear() entries() keys() values() size 
map方法：add() delete() set(key,value) get()（增删改查） entries() keys() values() size
```javascript
let set = new Set(['red', 'green', 'blue']);
for (let item of set.keys()) {
    console.log(item);
}
```
settimeout(fun,ms,arg1,arg2...argN) 第三个-最后一个参数会作为传递参数传给fun

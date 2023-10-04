---
title: 【基础】foreachforofforin
date: 2019-07-07 17:51:16
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---

# `for in`、`for of` 、`forEach` 区别
 `for of` 常用于异步遍历，`for in`、`forEach`、 `for` 多用于 同步遍历。

## 一、 基本用法

### 1. 遍历数组：

#### for of 遍历数组
```js
let nums = [1,2,3]
for(i of nums) {
    console.log(i)
} 
// 1,2,3 i为数组的值
```
#### for in 遍历数组
```
for(i in nums) {
    console.log(nums[i])
} 
// 1,2,3 i为数组下标
```
#### forEach 遍历数组

```js
nums.forEach((value,key)=>{
    console.log(value)
})
// 1,2,3
```
### 2. 遍历对象：

```js
let obj = {
  num:1,
  name:2,
  age:3
}
```
#### for in 遍历对象，不能遍历map、set，不报错，但由于没有key会返回undefined

```js
for(i in obj) {
  console.log(obj[i])
} // 1,2,3
```
#### for of、forEach 不能遍历对象

```js
for(i of obj) {
  console.log(i)
} // Uncaught TypeError: obj is not iterable
// for of 只能遍历有iterable（遍历器）接口的变量
```

**for of 只能遍历有iterable（遍历器）接口的变量，object没有遍历器接口。**

**原生具备 Iterator 接口的数据结构如下。Array、Map、Set、String、TypedArray 函数的 arguments 对象、NodeList 对象**

**forEach也不能遍历对象，但可以用Object.keys() Object.values() Object.entries(obj)去遍历对象**

**插一句：**set和map也有对应的values keys entries方法，返回的是各自类型的键组合、值组合、键值对类型组合，可以用for of 沙发垫

## 二、 forEach 和 for of 区别

## 1. 遍历对象方面

### for of 可以遍历的：

实现了迭代器接口，即具有`[Symbol.iterator]`方法，它们可以被`for...of`用于遍历。

### forEach可以遍历的：

数组和类数组对象，即：拥有```length```属性（这是我自己认为的，不一定对，反正暂时没出错）。

### forEach不能遍历而for of可以遍历的

`forEach`可以做的`for of`基本都可以做，但有些有`iterator`接口的变量，只能用`for of`来做。

如生成器函数（`generator`）、字符串的`Symbol`迭代器、自定义的迭代器对象等

### (1). 生成器函数（Generator Functions）：

```js
function* generateSequence() {
  yield 1;
  yield 2;
  yield 3;
}

// 使用for...of遍历生成器函数生成的序列
for (let value of generateSequence()) {
  console.log(value);
}
```

### (2). 字符h 串的Symbol迭代器：

```js
const str = "Hello";

// 使用for...of遍历字符串的Symbol迭代器
for (let char of str[Symbol.iterator]()) {
  console.log(char);
}
```

### (3). 自定义迭代器对象

```js
const customIterator = {
  values: [1, 2, 3],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => {
        if (index < this.values.length) {
          return { value: this.values[index++], done: false };
        } else {
          return { done: true };
        }
      },
    };
  },
};

// 使用for...of遍历自定义迭代器对象
for (let value of customIterator) {
  console.log(value);
}
```

## 2. for of 的异步遍历

如果使用`foreach`遍历处理异步代码不会等待异步代码的执行，一次输出所有异步结果
使用`forEach`打印一次性打印出了`1、4、9`

```js
function muti(num) {
	return new Promise((resolve) => {
		setTimeout(() => {
			resolve(num * num);
		}, 1000);
	});
}
var arr = [1, 2, 3];
arr.forEach(async (value, index, array) => {
	const val = await muti(value);
	console.log(val);
});
```
如果使用`for of`可以使异步按先后顺序执行
使用`for of`打印就可以实现每隔一秒进行打印`1、4、9`

```js

 function muti(num) {
		return new Promise((resolve) => {
			setTimeout(() => {
				resolve(num * num);
			}, 1000);
		});
 }
 var arr = [1, 2, 3];
 !(async function () {
		for (let a of arr) {
			const ans = await muti(a);
			console.log(ans);
		}
 })();
```

## 3. for of 异步遍历的例子

关于异步处理方面的区别，在使用`for...of`遍历可迭代对象时，可以使用`await`关键字来等待异步操作的完成。

而`forEach`方法本身是同步的，无法直接使用`await`等待异步操作。

以下是一个使用`for...of`进行异步处理的例子，其中包含了`await`关键字：

```js
async function processArray(arr) {
  for (let element of arr) {
    await doSomethingAsync(element);
  }
}

async function doSomethingAsync(element) {
  return new Promise((resolve) => {
    setTimeout(() => {
      console.log(element);
      resolve();
    }, 1000);
  });
}

const arr = [1, 2, 3];
processArray(arr);
```

在上述例子中，`processArray`函数使用`for...of`遍历数组，并通过`await`等待异步操作的完成。每个元素的处理会等待一秒钟，然后才会处理下一个元素。

相比之下，`forEach`方法无法直接实现类似的异步处理，因为它本身是同步的，无法等待异步操作的完成。
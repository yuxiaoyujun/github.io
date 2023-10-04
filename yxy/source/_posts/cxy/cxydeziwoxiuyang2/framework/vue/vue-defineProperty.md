---
title: '【vue】vue2和vue3分别实现数据侦听'
date: 2020-08-21 18:16:25
tags: vue
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## Object.defineProperty(obj, prop, descriptor)
在js原生对象中，实现数据侦听的方法就是defineProperty。
#### 使用示例如下：
```javascript
var obj = new Object();
var value;
Object.defineProperty(obj,'name',{
    get: function () {
        console.log(`get${obj['name']} `);
        return value;//必须return一个值，作为name属性的值
    },
    set: function (newvalue) {
        console.log('set it');
        value = newvalue;//同步把value的值进行更新
    }
});
console.log(obj);
console.log(obj.name);//get it
obj.name = ’aaaaa‘;//set it
console.log(obj.name);//get it
```
#### vue2中使用defineProperty中实现数据侦听
建立一个html文件，dom结构如下
```javascript
<div class="wrapper"></div>
```
编写侦听函数
```javascript
function observe(target) {
	if (typeof target !== "null" && typeof target !== "object") {
		return target;
	}
	for (let key in target) {
		defineReactive(target, key, target[key]);
	}
}
```
实现侦听的函数
```javascript
function defineReactive(target, key, value) {
	observe(value); // 使用递归，实现深度监听
	Object.defineProperty(target, key, {
		get() {
			return value;
		},
		set(newValue) {
			if (value !== newValue) {
				value = newValue;
				updateView(JSON.stringify(target));
			} else {
				console.log("value == newValue");
			}
		}
	});
}
```
当数据更改时，更新视图
```javascript
function updateView(value) {
	document.querySelector(".wrapper").innerText = value;
}

let serveObj = {
	name: "aaaccc",
	age: 11,
	other: {
		sex: "f"
	}
};
```
```javascript
observe(serveObj); 

serveObj.name = "sdfdsfsdfsd" // dom会实时改变innerHtml的内容 。
```
## proxy
拦截代理，拦截对应的操作后，进行处理，会新生成一个对象，然后通过proxy生产的对象去改变原对象。
#### 使用方法：
```javascript
let obj = { a: 1 };
let proxy = new Proxy(obj, {
	get(target, key) {
		return target[key];
	},
	set(target, key, value, proxy) {
		target[key] = value;
document.querySelector(".wrapper").innerText = JSON.stringify(proxy);
	}
});

proxy.b = 1;
obj // {a: 1, b: 1}
```

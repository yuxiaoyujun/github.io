---
title: 'Proxy略略略略记'
date: 2021-05-21 09:03:38
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、proxy
proxy就是一个拦截器，拦截了原来的对象之后，可以对这个对象进行自己的更改，也可以防止别人对对象进行某种更改，然后可以输出修改后的monitor，且不影响原对象。
```javascript
{
    let obj = {
        time: '2017-03-11',
        name: 'net',
        _r: 123
    }
```
### 1. proxy的声明
#### （1）可以声明后直接在handler里面写对象
```javascript
        let proxy = new Proxy(obj,handler)
        handler = {
             get () {},
             set () {},
             has () {},
         }
```
#### （2）下面这种声明方式
```javascript
	let proxy = new Proxy(obj,{
		get (target,key) {} // or get: function(target,key) {}
	})
```
## 2. get/set/has/等拦截方法
```javascript
    let monitor = new Proxy(obj,{
   //2. get方法：拦截对象属性的读取
        get (target,key) {
            if(target[key].replace)
                return target[key].replace('2017','2018')
            else 
                return target[key]
            // 一定要return
        },
        set (target,key,value) {
    // 3. set方法：拦截对象属性的更改
            if(key === 'name') {
                return target[key] == value
            }
        },
    // 4. has方法：对对象是否有该属性的拦截，拦截in 不拦截hasOwnProperty()
    // hasOwnProperty():判断一个对象属性里是否包含某个key，key为字符串,此方法不会去判断原型
    // in: key in obj //obj为对象 key为所要判断的字符串
        has (target,key) {
            if (key == '_r') {
                return false
            } else {
                return key in target
            }
        }
    // 5. construct() 拦截new命令
    // 6. deleteProperty() 拦截delete
    // 7. defineProperty() 拦截Object.defineProperty()
    // 8. ownKeys(target) 拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
    // 9. apply(target, object, args)：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
    })

    console.log(monitor.time) // 2018-03-11
    monitor._r = 'sdffsfsdfds'
    console.log(monitor._r)
    monitor.name = 'sdffsfsdfds'
    console.log(monitor.name)
    console.log('name' in monitor) // true
    console.log('_r' in monitor) // false
    console.log('_r' in monitor) // false
}
```
## 二、 reflect
#### （1） 名称和用法与proxy一样
#### （2） ES6 中将 Object 的一些明显属于语言内部的方法移植到了 Reflect 对象上（当前某些方法会同时存在于 Object 和 Reflect 对象上），未来的新方法会只部署在 Reflect 对象上。
#### （3） Reflect 对象对某些方法的返回结果进行了修改，使其更合理。
#### （4） Reflect 对象使用函数的方式实现了 Object 的命令式操作。
 为什么用reflect，要尽量避免用原生的Object方法，通过Reflect方法调用会比较合理和方便。
```javascript
{
    let obj = {
        time: '2017-03-11',
        name: 'net',
        _r: 123
    }

}

Reflect.get(obj,'name')  // 'net'
```

在vue3的源码中，原先的数据驱动视图的方法Object.defineProperty被替换成了Proxy来实现，在Proxy内部操作数据时就用了Reflect去调用对象方法。
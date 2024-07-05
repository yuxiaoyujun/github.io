---
title: 【nodejs】基础总结（一）
date: 2019-08-07 17:20:37
tags: 
  - node
  - lv2
categories: 程序员的自我修养
---

## 一、提前要了解的内容

Node API：fs url http util console 定时器 path 等等 模块

核心语法：javascript

## 二、Buffer 缓冲区

### 1. 什么是Buffer

Buffer是一个类似于Array的对象，它用于表示一段固定长度的字节序列。它用来处理二进制的数据。

### 2.  Buffer的特点

​    2.1. Buffer大小固定，且无法调整

​    2.2. Buffer性能较好，可以直接对计算机内存进行操作。

​    2.3. 每个元素的大小为1字节（byte），八个bit组成一个byte，一个bit就是一个0或1

**即：1字节能存储的最大空间为十进制0-255，二进制00000000-11111111，十六进制00-ff**

### 3. 使用方法

Buffer是nodejs的内置模块，使用时无需额外导入。

### 3.1 Buffer.alloc

alloc方法用来创建一段Buffer，它会将Buffer清零

```js
let buf = Buffer.alloc(10)
```

结果：

![](/images/image-20230710103925258.png)

注意：上面的 每个 00 都使用16进制表示。即，能存储的最大数为 00-ff ，也就是最大能存储0-255（十进制），00000000-11111111（二进制）

### 3.2 Buffer.allocUnsafe

allocUnsafe方法用来创建一段Buffer，它创建的Buffer可能包含旧的内存数据。所以不太安全。但创建速度比alloc快。

```js
let buf = Buffer.allocUnsafe(10000)
```

结果：

> <Buffer 00 00 00 00 00 00 00 00 11 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 11 ff ff ff 0a 00 00 00 ff ff ff ff 31 00 00 00 ff ff ff ff 37 00 00 00 00 00 ... 9950 more bytes>

### 3.3 Buffer.from

用于将一个字符串、数组、或字符串数组、或Buffer，转换为一段Buffer

```js
let str = 'abcdef'
let buf = Buffer.from(str)
```

结果：

><Buffer 61 62 63 64 65 66>

即：abcdef六个字母的的16进制unicode码

### 3.4 将buffer转换为字符串

上面说过，Buffer.from接收数组，将其转换为一段Buffer

```js
let buf = Buffer.from([97,98,99,100,101,102])  // 上面3.3的abcdef的十进制，61-65是16进制
buf.toString() // abcdef
```

### 3.5 修改和获取Buffer的值

获取：

```js
let buf = Buffer.from('abcdef')
console.log(buf[0].toString(16)) // 16进制的a为61，所以打印结果为61，这里的toString内部的参数为进制的数字，和上面的toString方法不一样。
```

修改：

```js
let buf = Buffer.from('abcdef')
buf[0] = 111 // unicode 十进制的o
console.log(buf.toString()) // obcdef
```

注：如果数字超过255，则超出位数会被舍弃掉。

如 256的16进制为100，那么存储为buffer会变成00

```js
let buf = Buffer.from('abcdef')
buf[0] = 256
console.log(buf[0]) // 结果为0，因为256的十六进制为100，100只保留00-ff，所以舍弃高位1，则为00。十进制为0
```








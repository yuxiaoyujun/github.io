---
title: '【第5章】dom、bom'
date: 2019-04-03 13:03:15
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 一、dom

#### 前言：dom的本质
html是一种特定的xml，html规定了语法，而xml可以自己规定标签
dom的本质是一个树形的数据结构
![](https://upload-images.jianshu.io/upload_images/20892169-2583ad059ce41ddb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1. document对象
###### （1）document对象集合
>all[]所有元素的集合 
anchors[]对所有href链接的引用，非常简单地返回文档中具有name属性值的所有锚点的集合。已被web标准删除。
applets对所有applets（小应用程序）对象的引用 
forms[]对所有forms的引用 
images[]对所有img的引用 
links[]对所有area的href的引用，<a>href

###### （2）document对象属性
```
cookie     
domain （域名，'www.baidu.com'）
lastModified（返回文档被最后修改的日期和时间。） 
referrer（从哪里跳转来的。返回"载入"当前文档的文档的 URL。这个比较常用）'https://www.baidu.com/from=844b/s?word=sdfsdf'
title（返回当前文档的标题。）
URL（当前文档url，和location.href的区别：URL不可设置，location.href可以设置）
```
###### （3）document对象方法
```
document.querySelector()
document.getElementById()	
document.getElementsByName()
document.getElementsByClassname()	
document.getElementsByTagName()	
document.createElement() 
document.createTextNode() 
document.createAttribute() 
document.open()	
```
#### 2. document.element
###### （1）节点操作
```
element.getElementsByTagName()
element.appendChild()  在元素末尾添加新结点
element.insertBefore(新节点，指定子节点) 在指定的已有的子节点之前插入新节点。
element.cloneNode(deep) 复制元素 要连后代一起复制请把 deep 参数设置 true，否则设置为 false。
element.hasChildNodes() 有子节点返回true
element.normalize() 合并元素中相邻的文本节点，删除空的文本节点
element.replaceChild(newchild,oldchild) 用newchild替换元素中的某个oldchlid了
element.removeChild(node) 删除元素中的某个子元素node
```
###### （2）节点属性操作
```
element.attributes 
//返回属性数组
element.getAttribute()
//返回元素节点的指定属性值。 <a href = 'abc'></a> a.getAttribute('href')--->abc
element.getAttributeNode(name) let atr = document.querySelector('a').getAttributeNode('href')
//返回指定的属性节点。 <a href = 'abc'></a> a.getAttribute('href')--->href = 'abc' a.getAttribute('href').value ---> abc a.getAttribute('href').name--->href
element.hasAttribute(name)
// 如果元素拥有指定属性，则返回true，否则返回 false。
element.hasAttributes()
// 如果元素拥有属性，则返回 true，否则返回 false。
element.setAttribute(name,value) a.setAttribute('href','http://xxx.cn')
element.setAttributeNode()//用来设置已经创建的attribute节点
/*
    // 用法
    let div = document.createElement('div')
    let attr = document.createAttribute('attr')
    attr.value = '1'
    div.setAttributeNode(attr)
    div // <div attr = "1"></div>
*/
let newAtrNode = document.createAttribute('class') newAtrNode.nodeValue='abc' newNode.setAttributeNode(newAtrNode)
element.removeAttribute(name) 
element.removeAttributeNode() // let atr = document.querySelector('a').getAttributeNode('href') document.querySelector('a').removeAttribute(atr)
element.style.xxx 获取元素内联的样式(style = "??????")，也可以通过它进行设置
element.className
```
###### （3）节点属性
```
element.childNodes  返回元素子节点的nodelists
element.className 返回元素类名
element.innerHTML
element.id	
element.firstChild 返回元素的首个子元素
element.lastChild 返回元素的最后一个子元素
element.firstElementChild element.lastElementChild 返回元素的最后一个子标签元素
element.nodeType 1 元素 2 属性 3 文本 8 注释
element.nodeValue 元素节点的 nodeValue 是 undefined 或 null 文本节点的 nodeValue 是文本自身 属性节点的 nodeValue 是属性的值
element.nextSibling 当前节点的下一个节点（同一树层级）
element.previousSibling 当前节点的上一个节点（同一树层级）
element.innerHTML element.innerText
element.clientHeight element.clientWidth 返回元素的可见高度可见宽度（要减去滚动条滚动过的高度） 
element.offsetWidth element.offsetHeight 返回元素的高度宽度（元素本身的高度宽度，包括边框，不受滚动条影响）
element.offsetTop element.offsetLeft // 元素距上层元素的高度/宽度，不受滚动条影响的
element.scrollTop  element.scrollLeft // 滚动的高度/宽度
element.offsetParent 返回元素的偏移容器，不会是div
```
![](https://upload-images.jianshu.io/upload_images/20892169-dd392939dc6498e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3.attribute 在属性节点上的可用的属性或方法
```
attr.isId 属性是id返回true反之false
attr.name 属性名称 btn.attributes[0].name，只读
attr.value 属性值 btn.attributes[0].value
nodemap就是element.attributes ~
nodemap.getNamedItem() // // 通过名称获取指定属性
nodemap.item(num) // 返回nodemap的指定位置属性
nodemap.length	// 返回nodemap长度
nodemap.removeNamedItem(num) // 移出指定位置属性
nodemap.setNamedItem() // 通过名称设置指定属性

// var btn=document.getElementsByTagName("h1")[0];
// var typ=document.createAttribute("class");
// typ.nodeValue="democlass";
// btn.attributes.setNamedItem(typ);
```
#### 4.event
###### （1）基本写法
``` 
document.querySelector('body').addEventListener('click',()=>{
    alert(1)
})
```
###### （2）事件类型
```
onabort 图像的加载被中断。
onreset	重置按钮被点击。
onselect	文本被选中。
onunload	用户退出页面。
onchange	域的内容被改变。(input textarea)
onkeydown onkeypress	onkeyup	
onload 一张页面或一幅图像完成加载。
onmousedown onmousemove onmouseout onmouseover onmouseup

// altKey ctrlKey metaKey shiftKey clientX clientY screenX screenY clientX clientY
// currentTarget target	type(返回当前 Event 对象表示的事件的名称。)
// preventDefault()	阻止默认事件 stopPropagation()停止冒泡

// 捕获比冒泡先执行，从内到外冒泡，从外到内捕获 addEventListener第三个参数默认为false 即冒泡 true 即捕获
```
#### 二、bom
（1）window下
###### history 
    back()、forward()、go(n)
###### location
以http://localhost:8080/axiospage?name=aaa&age=12#abc举例
```
hash(#后)   //'#abc'
host  // localhost:8080
hostname  //localhost
href   //'http://localhost:8080/axiospage?name=aaa&age=12#abc'
pathname（纯路径无#无?）'/axiospage'
port    //'8080'
protocol   //'http:'
search(?后)   //'?name=aaa&age=12'
assign(url)替换新url，会存储在当前历史中，和href作用一样。
reload()重新载入，为true时绕过缓存载入文档，为false或无参数则从缓存加载大概
replace(newUrl)	(不会存储在历史跳转中，替换了当前)
```
###### Navigator：
Navigator下的属性基本是浏览器的各种信息，如userAgent返回由客户机发送服务器的 user-agent 头部的值。
###### 其他常用的：
```
pageXOffset、pageYOffset	 
innerheight、innerwidth（无border margin）	
outerheight、outerwidth(加border margin) 
self（相当于window）
Screen
返回屏幕信息，分辨率、大小、英寸、像素 availHeight	availWidth	deviceXDPI	deviceYDPI	height	width	pixelDepth	等等
requestAnimationFrame
```
#### 面试题：
###### 如何提高dom性能？
(1) 避免频繁的dom操作
(2) 对dom查询做缓存 
(3) 将dom频繁操作改为一次性操作
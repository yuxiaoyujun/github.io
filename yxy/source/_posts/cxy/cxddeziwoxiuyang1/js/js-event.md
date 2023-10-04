---
title: '【第6章】事件捕获与冒泡相关'
date: 2019-04-10 12:13:35
tags:
  - js
  - lv1
categories:
  - 程序员的自我修养
---
<meta name="referrer" content="no-referrer">

## 描述事件冒泡的流程
事件冒泡基于树形dom结构，事件可以一级一级的向上冒泡，若有多个事件，先响应内层，再响应外层。若一个事件在最外层绑定的，在最内层上依旧能监听到。

## 事件冒泡和事件捕获 
##### 事件冒泡：事件可以一级一级的向上冒泡，若有多个事件，先响应内层，再响应外层。
```javascript
event.stopPropagation() // 阻止向上冒泡，事件只在当前层生效。

event.preventDefault() 
// 阻止默认事件，默认事件包括
//  a标签的默认跳转
//  鼠标右键菜单事件
//  submit表单提交事件

event.target // 实际触发事件是在哪个元素上

event.target.dataset 
// 可以获取元素上的data-xxx属性，如<div data-a = "1"></div>，那么event.target.dataset.a == 1

event.currentTarget // 事件绑定在哪个元素上

event.target.matches(selector) --> // 判断是不是该元素，选择符可以是类、id、也可以是元素标签
```
##### 事件捕获：从外向内触发事件，很少用于开发。
xxx.addEventListener('xxx',function(){},true) 第三个参数为true时，则触发事件捕获。

## 事件代理
像瀑布流，无限加载图片，由于元素太多不好每一个元素都绑定事件，所以就将事件绑定在这些图片的父元素上，通过事件冒泡去相应事件
好处是代码简洁，减少浏览器占用，但不要滥用。


## 面试题 
#### 编写一个通用的事件监听函数 
为了解决事件代理（如瀑布流需要每个图片都有一个点击事件）将瀑布流事件绑定在父元素上，当点击父元素时，可以激发子元素的事件
```javascript
function bindEvent(element,eventType,selector,fn) {
    if(fn == null) { 
// 第三个参数是触发子元素事件的可选，如果没有子元素，那么第三个参数应该是fn，所以赋值一

        fn = selector
        selector = null
    }
    element.addEventListener(eventType,e => {
        let target = e.target
        if (selector) {
            if (target.matches(selector)) {
                fn.call(target,e)
            }
        } else { // 不需要代理
            // window.fn.call(target,e) ，就想，call这个函数实际上在call的对象上面加了一个一模一样的fn方法，然后变成了target.fn
            fn.call(target,e) // 用call是因为下面bindEvent实际上是在window下，所以里面的this是指向window的，现在是需要将this指向调用addEventListener的event
        }
    })
}
```
```javascript
// 我写的，我把selector移到第四个参数了，这样可以省一步判断
function bindEvent(element,eventType,fn,selector) {
    element.addEventListener(eventType,(event)=>{
        let target = event.target
        if(selector) {
            if(target.matches(selector)) {
                fn.call(target,event)
            }
        } else {
            fn.call(target,event)
        }
    })
}
 let btn = document.querySelector('btn')
 bindEvent(btn,'click','child_div',function(e){
    e.preventDefault() 
    alert(this.innerHTML) 
 })
```
---
title: 面试题整理（持续更新）
date: 2021-03-20 17:58:32
tags: 
 - lv3
categories:
  - 程序员的自我修养
---

## js
### call绑定null
call和apply第一个参数为null/undefined，函数this指向全局对象
### 跨域解决方案
cors和jsonp
### jsonp为什么会自动执行？
又不是请求而是script标签当然会自动执行。。

## css
### 重排(reflow)和重绘(repaint)

+ 重绘：某些元素的外观被改变，例如：元素的填充颜色
+ 重排：重新生成布局，重新排列元素。

**重绘不一定导致重排，但重排一定会导致重绘。**

### 如何提高dom渲染性能？

减少重排次数：
+ 集中改变dom
+ 分离读写操作
+ 使用display: none之后再进行 dom的修改，就会避免重排
+ 使用absolute或fixed脱离文档流，修改子元素就不会影响父元素以上的元素重排
+ 固定会影响父元素重排的css，再去操作子元素的重排和重绘，这样就不会影响父元素之外的其他元素
+ 优化动画，比如translate: 1s改为更长的时间，减少帧数。

## 网络
### websocket
### http缓存策略
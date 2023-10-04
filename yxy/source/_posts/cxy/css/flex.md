---
title: 【flex】简单总结
date: 2023-03-15 18:02:02
tags:
  - css
  - lv1
categories:
  - 程序员的自我修养
---

## 一、容器属性

#### 1. justify-content

`flex-start`（默认值）：左对齐
`flex-end`：右对齐
`center`： 居中
`space-between`：两端对齐，项目之间的间隔都相等。
`space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

#### 2. align-items

`flex-start`：交叉轴的起点对齐。
`flex-end`：交叉轴的终点对齐。
`center`：交叉轴的中点对齐。
`baseline`: 项目的第一行文字的基线对齐。
`stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#### 3. flex-flow 

`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

#### 4. flex-wrap 换行

`nowrap`（默认）：不换行。
`wrap`：换行，第一行在上方。
`wrap-reverse`：换行，第一行在下方。

#### 5. align-content

多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

#### 6. flex-direction
`row`（默认值）：主轴为水平方向，起点在左端。
`row-reverse`：主轴为水平方向，起点在右端。
`column`：主轴为垂直方向，起点在上沿。
`column-reverse`：主轴为垂直方向，起点在下沿。

## 二、子元素属性

#### 1. order 排列顺序
数字

#### 2. flex-grow 剩余空间的项目放大比例
数字越大拉伸越严重

#### 2. flex-shrink 剩余空间的项目缩小比例
数字越大压缩越严重

#### 3. flex-basis 分配剩余空间前，该子元素的占据尺寸。
优先级高于flex-grow和flex-shrink
可以为具体数值，比如flex-basis: 150px
#### 4. flex 
2/3/4的简写

#### 5. align-self 
子元素的默认对齐方式
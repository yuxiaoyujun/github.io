---
title: 【BFC】BFC
date: 2023-04-03 11:12:24
tags: 
  - lv1
  - css
categories: 程序员的自我修养
---

## BFC概念

BFC是“块级格式化上下文”的缩写，用于控制文档中块级元素的布局和定位。在一个BFC中，元素按照一定的规则进行排列，不会影响到BFC外部的元素。BFC的常见特征包括：

+ 内部的盒子会在垂直方向上一个接一个地放置，因此不会重叠。
+ 在BFC中，每个盒子的左边和右边外边距（margin）不会和其他元素的外边距折叠（collapse）。
+ BFC可以包含浮动元素，因此可以避免浮动元素对其他元素的影响。
+ BFC的高度可以自适应其内部元素的高度，不会塌陷（collapse）。

通过理解和应用BFC，可以更好地掌控页面的布局和样式，解决一些常见的排版问题，比如清除浮动、避免外边距折叠等。

## BFC的实际运用场景

#### 清除浮动：
当一个父元素包含了一个浮动元素时，父元素可能会因为子元素浮动而导致高度塌陷。这时可以在父元素上创建一个新的BFC来解决这个问题，例如：
```html
<div class="parent">
  <div class="float"></div>
</div>
```

```css
.parent {
  overflow: hidden; /* 创建新的BFC */
}
.float {
  float: left;
}
```

#### 避免外边距折叠
当两个相邻的元素的外边距发生重叠时，可以在其中一个元素上创建一个新的BFC来避免这个问题，例如：
```html
<div class="container">
  <div class="child1"></div>
  <div class="child2"></div>
</div>
```

```css
.container {
  overflow: hidden; /* 创建新的BFC */
}
.child1 {
  margin-bottom: 20px;
}
.child2 {
  margin-top: 30px;
}
```
#### 自适应布局
在一个父元素中，如果有一些子元素的高度不固定，可以在父元素上创建一个新的BFC，从而实现自适应布局，例如：
```html
<div class="parent">
  <div class="child"></div>
  <div class="child"></div>
  <div class="child"></div>
</div>
```

```css
.parent {
  overflow: hidden; /* 创建新的BFC */
}
.child {
  float: left;
  width: 33.33%;
}
```

#### 解决文本环绕问题
当需要在一个元素周围放置文本时，可以在该元素上创建一个新的BFC，从而实现文本环绕的效果，例如：
```html
<div class="image">
  <img src="image.jpg">
</div>
<p>这里是一些文本，会围绕着上面的图片环绕。</p>
```

```css
.image {
  float: left; /* 让图片浮动 */
  margin-right: 10px; /* 调整图片与文本的间距 */
  overflow: hidden; /* 创建新的BFC */
}
```

#### 其他可以创建bfc的例子

上面列举的多数还是使用overflow: hidden时，这种BFC的创建方式是很常用来解决布局问题的。实际上还有很多别的方法会创建BFC。

#### + 使用float属性创建新的BFC：
```html
<div class="parent">
  <div class="float"></div>
</div>
```
```css
.float {
  float: left;
}

.parent::after {
  content: "";
  display: block;
  clear: both;
}
```
#### + 使用position属性创建新的BFC：
```html
<div class="parent">
  <div class="absolute"></div>
</div>
```

```css
.absolute {
  position: absolute;
  top: 0;
  left: 0;
}

.parent {
  position: relative;
  height: 300px;
  background-color: #eee;
  overflow: hidden; /* 创建新的BFC */
}

```

#### 使用display创建BFC

使用一些display属性值也可以创建新的BFC，例如
`display: table-cell`
`display: table-caption`
等。


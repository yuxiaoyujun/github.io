---
title: 【复习整理】sass简单整理
date: 2023-02-06 01:28:59
tags:
  - css
  - lv2
categories:
  - 程序员的自我修养
---

  <meta name="referrer" content="no-referrer">


## 思维导图：
![](https://upload-images.jianshu.io/upload_images/20892169-561721a3995a9bea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 一、css拓展功能
### 1. 嵌套规则
Sass 内层的样式可以将它外层的选择器作为父选择器
```css
#main {
  width: 97%;

  p, div {
    font-size: 2em;
    a { font-weight: bold; }
  }

  pre { font-size: 3em; }
}
```
编译为

```css
#main {
  width: 97%; }
  #main p, #main div {
    font-size: 2em; }
    #main p a, #main div a {
      font-weight: bold; }
  #main pre {
    font-size: 3em; }
```
### 2. 父选择器（&）
```css
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```
编译为
```css
a {
  font-weight: bold;
  text-decoration: none; }
  a:hover {
    text-decoration: underline; }
  body.firefox a {
    font-weight: normal; }
```
### 3. 属性嵌套（：）
```css
.funky{
  font: {
    font-family: fantasy;
    font-size: 30em;
    font-weight: bold; 
  }
}
```
编译为
```css
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```
### 4. 占位符选择器（%foo）
#### 4.1 @extend
比如，你有一些按钮需要设置样式。这些按钮都具有相同的特征，只是每个按钮的颜色不同。
你可以创建一个 .button 类以包含按钮的所有通用代码，然后为每个类创建额外的类，为了添加背景颜色之前扩展 .button 类。
```css
.button {
  font-size: 1rem;
  text-decoration: none;
  color: #FFFFFF
}
```
```css
.button-red {
  @extend .button;
  background：#900；
}
.button-green {
  @extend .button;
  background：#090；
}
```

#### 4.2 占位符选择器
写法和class(.)以及id(#)是一致的，但前面的符号为%，必须配合@extend使用。
它取代以前 CSS 中的基类造成的代码冗余的情形，因为 %placeholder 声明的代码，如果不被 @extend 调用的话，不会产生任何代码，
```css
%demo1 {
  border-radius: 10px;
}
%demo2 {
  background-color: #ff0000;
}
div {
  @extend %demo1;
  @extend %demo2;
}
```
编译后：
```css
div {
  border-radius: 10px;
  background-color: #ff0000
}
```
### 5. 注释/* */与 //
没啥说的
多行注释会被完整输出到编译后的文件，单行注释则不会。

### 6. SassScript
#### 6.1 Interactive Shell
用来测试SassScript的功能，在命令行中输入`sass -i`燃弧输入想要的测试SassScript查看输出结果
```css
$ sass -i
>> "Hello, Sassy World!"
"Hello, Sassy World!"
>> 1px + 1px + 1px
3px
>> #777 + #777
#eeeeee
```
#### 6.2 变量$
```css
  $width: 5em;
  .main {
    width: $width;
  }
```
变量有块级作用域，如果想转换为全局作用域，需要添加!global声明
```css
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```
编译为：
```css
#main {
  width: 5em;
}
#sidebar {
  width: 5em;
}
```
#### 6.3 数据类型
sass支持6种（喵？）数据类型
+ 数字，`1, 2, 3, 10px`
+ 字符串，`"foo", 'bar', baz`
+ 颜色，`blue，#04a3f9`
+ 布尔，`true，false`
+ 空值，`null`
+ 数组，`$list: (1.5em 1em) `或`$list: (1.5em, 1em)`，可以去掉括号
+ maps，相当于object，`(key1: value1,key2: value2)`

#### 6.4 运算
\+ - * / %
颜色也可以运算
#### 6.5 函数
sass提供了一些函数，详细看文档吧。
#### 6.6 自定义函数
```css
$grid-width: 40px;
$gutter-width: 10px;
@function grid-width($n) {
  @return $n * $grid-width + ($n - 1) * $gutter-width;
}
#sidebar { width: grid-width(5); }
```
#### 6.7 插值语句 #{}
想要在选择器中使用变量的话，就可以使用插值语句。
``` css
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
```

#### 6.8 type-of()检测数据类型
可以使用type-of()检测变量的类型
```css
$variable: "Sass is awesome";
$variable2: Sass is awesome;
```
```
>> type-of($variable)
"string"
```

### 7. 指令
#### 7.1 @import
@import "foo.scss"
### 8. 控制指令
#### 8.1 @if @else if @else
```css
$type: monster;
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```
编译为：
```
p {
  color: green; }
```
#### 8.2 @for
@for from through
@for from to
```css
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
```
```css
.item-1 {
  width: 2em; }
.item-2 {
  width: 4em; }
.item-3 {
  width: 6em; }
```
**from...through 与 from...to 的区别:**

from 1 through 3 表示1/2/3
from 1 to 3 表示 1/2，不包括3
#### 8.3 @each
@each in
```css
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```
编译为
```css
.puma-icon {
  background-image: url('/images/puma.png'); }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); }
.egret-icon {
  background-image: url('/images/egret.png'); }
.salamander-icon {
  background-image: url('/images/salamander.png'); }
```
## 九、输出格式
### 1. 命令
```sass :style option```
或
```sass --style option```
### 2. 常用的style值
#### :nested
Nested （嵌套）样式是 Sass 默认的输出格式，能够清晰反映 CSS 与 HTML 的结构关系。选择器与属性等单独占用一行，缩进量与 Sass 文件中一致，每行的缩进量反映了其在嵌套规则内的层数。当阅读大型 CSS 文件时，这种样式可以很容易地分析文件的主要结构。
```css
#main {
  color: #fff;
  background-color: #000; }
  #main p {
    width: 10em; }

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline; }
```
#### :expanded
Expanded 输出更像是手写的样式，选择器、属性等各占用一行，属性根据选择器缩进，而选择器不做任何缩进。
```css
#main {
  color: #fff;
  background-color: #000;
}
#main p {
  width: 10em;
}

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline;
}
```
#### :compact
Compact 输出方式比起上面两种占用的空间更少，每条 CSS 规则只占一行，包含其下的所有属性。**嵌套过的选择器在输出时没有空行，不嵌套的选择器会输出空白行作为分隔符。**
```css
#main p { width: 10em; }

.huge { font-size: 10em; font-weight: bold; text-decoration: underline; }
```
#### :compressed
Compressed 输出方式删除所有无意义的空格、空白行、以及注释，力求**将文件体积压缩到最小**，同时也会做出其他调整，比如会自动替换占用空间最小的颜色表达方式。
```
#main{color:#fff;background-color:#000}#main p{width:10em}.huge{font-size:10em;font-weight:bold;text-decoration:underline}
```
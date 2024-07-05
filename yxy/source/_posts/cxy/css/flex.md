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

`flex-grow: 0` 表示项目剩余空间不分配。

`flex-grow: 1`: 表示项目剩余空间按比例分配至自适应父元素宽度。

数字越大拉伸越严重

#### 2. flex-shrink 剩余空间的项目缩小比例

子元素未超出空间，设置无效。

当子元素总宽度超出了容器时，收缩比例。

`flex-shrink: 0` 表示项目即使超出也不压缩。

`flex-shrink: 1`: 表示项目按比例压缩至自适应父元素宽度。

数字越大压缩越严重

#### 3. flex-basis 分配剩余空间前，该子元素的占据尺寸。
优先级高于`flex-grow`和`flex-shrink`

若`flex-basis: 0`，则该元素按自身内容自适应，无视被设置的width。

可以为具体数值，比如`flex-basis: 150px`

#### 4. flex 
2/3/4的简写

设置`flex：1`（相当于`flex: 1 0 0`或`flex-grow: 1`），可以让项目自适应到父元素的宽度。

设置`flex: 0`，可以让子元素无视被设置的宽度，自适应到内部元素的宽度。

#### 5. align-self 
子元素的默认对齐方式

## 三、其他

假设有这样的div结构：

```html
<div id="content">
  <div class="box" style="background-color:red;">A</div>
  <div class="box" style="background-color:lightblue;">B</div>
  <div class="box" style="background-color:yellow;">C</div>
  <div class="box1" style="background-color:brown;">D</div>
  <div class="box1" style="background-color:lightgreen;">E</div>
</div>
```

1. 默认情况下，子元素的宽度由他们的内容决定，即使在设置了父元素的总宽度的情况下，也只是根据内容自适应。

![](/images/image-20231007212727376.png)

```css
#content {
	display: flex;
	width: 500px;
	height: 300px;
	background-color: #abcdef
}
```

2. 默认情况下，若子元素总宽度超出父元素，则自适应收缩到父元素宽度。

   若有单独的子元素有设置宽度，则按比例收缩

![](/images/image-20231007214011112.png)

```css
#content {
  display: flex;
  width: 500px;
  height: 300px;
  background-color: #abcdef
}

#content div {
  width: 120px;
  height:120px;
  border: 3px solid rgba(0,0,0,.2);
}
#content .box {
	width: 200px
}
```

3. 若子元素宽度未超出父元素，则`flex-shrink`失效。

   ![](/images/image-20231007214254982.png)

```css
#content {
  display: flex;
  width: 500px;
	height: 300px;
	background-color: #abcdef
}

#content div {
	width: 80px;
	height:80px;
	flex-shrink: 10;
  border: 3px solid rgba(0,0,0,.2);
}
```

4. 设置`flex: 0`，可以让子元素无视被设置的宽度`width`，自适应到内部元素的宽度。（相当于`flex-basis: 0`）

   ![](/images/image-20231007215153305.png)

```css
#content {
  display: flex;
  width: 500px;
	height: 300px;
	background-color: #abcdef
}

#content div {
	width: 20px;
	height: 20px;
  flex:0;
  border: 3px solid rgba(0,0,0,.2);
}
#content .box {
	width: 90px
}
```

5. `flex-basis`的优先级大于`width`

   ![](/images/image-20231007215724622.png)

```css
#content {
  	display: flex;
  	width: 500px;
		height: 300px;
		background-color: #abcdef
}

#content div {
		height: 20px;
  	border: 3px solid rgba(0,0,0,.2);
}
#content .box {
		width: 90px;
		flex-basis: 200px;
}
```

6. `flex-shrink: 0`，表示无视容器宽度，被设置的子元素永不收缩，即使超出父容器。

![](/images/image-20231007220053014.png)

```css
#content {
  display: flex;
  width: 500px;
	height: 300px;
	background-color: #abcdef
}

#content div {
	flex-basis: 200px;
	height: 20px;
  border: 3px solid rgba(0,0,0,.2);
}
#content .box {
	width: 90px;
	flex-shrink:0;
}
```

7. `flex-basic`、`flex-grow`和`flex-shrink`的优先级：

   要注意一点，`flex-basis`是**最先**被设置的属性，但这并不代表生效的优先级最高，`flex-basis`优先级必定大于`flex-grow`和`width`，但和`flex-shrink`比起来，优先级不一定是最高的。所以说并不是`flex-basic`设置后，`flex-shrink`就无效了。

   - 项目首先会按照`flex-basis`的值进行设置，优先级高于`flex-grow`与`width`
   - 如果剩余空间不足以容纳所有项目的 `flex-basis` 尺寸之和，并且某些项目的 `flex-shrink` 值大于 `1`，那么这些项目会按照其 `flex-shrink` 值收缩，以适应剩余空间。
   - 如果剩余空间仍然不足，那么项目的 `flex-basis` 属性值将影响哪些项目更容易受到收缩。具体来说，`flex-basis` 的值较大的项目更不容易受到收缩，因为它们需要更多的剩余空间才能保持其初始尺寸。

8. 若`flex-basis: 0`，则该元素按自身内容自适应，无视被设置的width。（相当于`flex: 0`）

   ![](/images/image-20231007224532238.png)

   ```css
   #content {
     display: flex;
     width: 500px;
   	height: 300px;
   	background-color: #abcdef
   }
   
   #content div {
   	flex-basis: 0;
   	height: 20px;
     	border: 3px solid rgba(0,0,0,.2);
   }
   #content .box {
   	width: 100px;
   }
   ```

   
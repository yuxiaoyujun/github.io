---
title: 【移动端】自适应方案
date: 2023-03-20 17:53:43
tags: 
  - lv1
  - css
categories:
  - 程序员的自我修养
---
## 基本概念：
**设备独立像素（DIP）**：逻辑像素，同樣嘅屏幕大小DIP都喺一樣嘅。
**物理像素（pt）**：分辨率，屏幕一行同埋一列分别有幾多像素点leigaa。
**设备像素比DPR(Device Pixel Ratio)**：DPR = 物理像素 / 设备独立像素，它通常与视网膜屏(Retina 屏)有关。

## 适配方案
#### 1. px + 自适应缩放（viewport）
适用于设计给的设计稿只需要在各种屏幕等比缩放的情况。
设置meta viewport的width为设计稿给出的width，使设计稿在不同屏幕上只需要自适应缩放。
```html
<meta
	name="viewport"
	content="width=750,user-scalable=no" />
```
如上，就是设计给的设计稿为750px，CSS单位使用px即可。

<hr>

这个方法写法简便，设计稿是多少就写多少，开发速度很快。可能会有部分特别特别低版本的手机不兼容。
这个方法有一个很大的缺点，就是缩放全局等比缩放，有些不想缩放的（比如部分1px边框、文字等）也会因为它的影响而等比缩放。
#### 2. rem适配方案
其实属于viewport的过渡方案，所以其实它和第一种viewport方案是一致的。只不过是早年对viewport的兼容性不好，所以早年是使用rem+js去实现自适应缩放。
**rem**：‘The font size of the root element’，就是以根元素 的字体大小为基本单位，是一种相对单位。
rem 适配的原理就是以 html 的 font-size 大小为基本单位来布局。
**em**：另一种相对单位，它相对于该元素的本身的 font-size 值来计算。
em是可以嵌套计算的，所以容易导致一些奇怪的问题。
```js
// 获取文档元素
var docEl = doc.documentElement;
// 定义当窗口发生变化时的事件
var resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
// 定义重新计算文档元素尺寸的函数
var recalc = function () {
    // 获取文档元素宽度
    var clientWidth = docEl.clientWidth;
    if (!clientWidth) return;
    // 设置文档元素字体大小
    docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';
  // 基于屏幕为750px的宽度进行缩放，设置成别的也ok的
};
// 监听窗口变化事件
if (!doc.addEventListener) return;
win.addEventListener(resizeEvt, recalc, false);
// 监听文档加载完成事件
doc.addEventListener('DOMContentLoaded', recalc, false);
```
假设屏幕750px，设置根元素为100px，即1rem=100px。
那么24px的字体，就是0.24rem。
在不同屏幕上的缩放就是
实际屏幕大小/750px = 实际根元素字体大小/100px
所以实际根元素字体大小 = 实际屏幕大小*100px/750px
设置根元素100px是为了好计算，直接除以一百就算出来了。设置个奇奇怪怪的值当然也可以按这个比例算。就是可以但没必要~

这个方法的兼容性很好，在遇到不想缩放的元素时，也可以用px进行设置。就是需要同时用js进行各个屏幕适配，比较麻烦。
#### 3. vw、vh适配

vh、vw方案：将视觉视口宽度 window.innerWidth和视觉视口高度 window.innerHeight 等分为 100 份。
vw(Viewport's width)：1vw等于视觉视口的1%
vh(Viewport's height) :1vh 为视觉视口高度的1%
vmin : vw 和 vh 中的较小值
vmax : 选取 vw 和 vh 中的较大值

如果设计稿是750px宽度。
如果视觉视口为750px，那么1vw = 1% = 7.5px，这时UI给定一个元素的宽为75px（设备独立像素），我们只需要将它设置为 75px / 750px = 10% = 10vw。
就是酱紫拉~它和屏幕大小没啥关系的。就是百分比

#### 4. 自动计算的插件
实际开发过程当中，计算每一个dom元素的vw或者rem是很麻烦的，所以可以使用postcss-px-to-viewport去自动转换。
```js
module.exports = {
  plugins: {
    // ...
    'postcss-px-to-viewport': {
      // options
    }
  }
}
```
##### 配置参数
默认参数:
```json
{
  unitToConvert: 'px',
  viewportWidth: 320,
  unitPrecision: 5,
  propList: ['*'],
  viewportUnit: 'vw',
  fontViewportUnit: 'vw',
  selectorBlackList: [],
  minPixelValue: 1,
  mediaQuery: false,
  replace: true,
  exclude: undefined,
  include: undefined,
  landscape: false,
  landscapeUnit: 'vw',
  landscapeWidth: 568
}
```

**unitToConvert (String)** 需要转换的单位，默认为"px"
**viewportWidth (Number)** 设计稿的视口宽度
**unitPrecision (Number)** 单位转换后保留的精度
**propList (Array)** 能转化为vw的属性列表
**viewportUnit (String)** 希望使用的视口单位
**fontViewportUnit (String)** 字体使用的视口单位
**selectorBlackList (Array)** 需要忽略的CSS选择器，不会转为视口单位，使用原有的px等单位。
**minPixelValue (Number)** 设置最小的转换数值，如果为1的话，只有大于1的值会被转换
**mediaQuery (Boolean)** 媒体查询里的单位是否需要转换单位
**replace (Boolean)** 是否直接更换属性值，而不添加备用属性
**exclude (Array or Regexp)** 忽略某些文件夹下的文件或特定文件，例如 'node_modules' 下的文件
**landscape (Boolean)** 是否添加根据 **landscapeWidth** 生成的媒体查询条件 @media (orientation: landscape)
**landscapeUnit (String)** 横屏时使用的单位
**landscapeWidth (Number)** 横屏时使用的视口宽度

当然，less、sass也有对此自定义解决的方案，可以自行查看文档。
### 5. Viewport 方案（推荐）
> 由于 viewport 单位得到众多浏览器的兼容，lib-flexible 这个过渡方案已经可以放弃使用，不管是现在的版本还是以前的版本，都存有一定的问题。建议大家开始使用 viewport 来替代此方案。

vw 适配方案的流程：

1.  meta 标签设置 viewport 宽度为屏幕宽度；
2.  开发环境配置 [postcss-px-to-viewport](https://www.npmjs.com/package/postcss-px-to-viewport) 或者类似插件；
3.  根据设计稿写样式，元素宽高直接取设计稿宽高即可，单位为 px，插件会将其转换为 vw；
4.  段落文本也按照设计稿写，单位为px，不需要转换为 vw；
#### 6. media媒体查询
有时候用户需要的更大的屏幕上更多的内容，而不是更大的字，所以在pc、平板一侧主要用到的兼容方式是media媒体查询。
```css
@media screen and (max-width: 300px) {
    body {
        background-color:lightblue;
    }
}
```
使用 @media 查询，你可以针对不同的媒体类型定义不同的样式。

你也可以针对不同的媒体使用不同样式文件 :
```html
<!-- 宽度大于 900px 的屏幕使用该样式 -->
<link rel="stylesheet" media="screen and (min-width: 900px)" href="widescreen.css">
<!-- 宽度小于或等于 600px 的屏幕使用该样式 -->
<link rel="stylesheet" media="screen and (max-width: 600px)" href="smallscreen.css">
```
因为这里主要讨论的是移动端，所以media媒体查询的用法不详述了。
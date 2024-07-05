---
title: 【ai繪畫】Midjourney 之 Param（参数）
date: 2023-04-10 19:06:10
tags: 
  - ai繪畫
  - Midjourney
categories: 繪畫
---
## 一、命令列表

### **1. `--aspect`或 `--ar`**

纵横比。如`--ar 16:9`就是纵横比16:9

### **2. `--chaos <number 0–100>`**

`--chaos <number 0–100>` 改变结果的多样性。较高的值会产生更多不寻常的结果。

`--chaos`接受值 0–100。
默认`--chaos`值为 0。

### **3. `--no`**

`--no plants` 会尝试从图像中移除植物。

### **4. `--quality <.25>`或 `--q <.25>`**

`--quality <.25, .5, 1, or 2>`, 或 `--q <.25, .5, 1, or 2>` 您要花费多少时间进行渲染。默认值为 1。值越高渲染结果越质量越高，值越低渲染结果越质量越低。

### **5. `--repeat`或`--r`**

`--repeat <1–40>`, or `--r <1–40>` 从单个提示创建多个作业。`--repeat`对于多次快速重新运行作业很有用。

>`--repeat`适用于 Standard 和 Pro[订阅者，](https://docs.midjourney.com/plans)
>`--repeat` Standard 订阅者接受 2–10 的值。
>`--repeat`Pro 订阅者接受值 2–40。
>该参数只能在Fast GPU 模式`--repeat`下使用。 使用作业结果上的重做（重新滚动）🔄 按钮只会重新运行提示一次。



### **6. `--seed`或`--sameseed`**

Midjourney 机器人使用种子编号创建视觉噪声场，如电视静态，作为生成初始图像网格的起点。种子编号是为每个图像随机生成的，但可以使用 --seed 或 --sameseed 参数指定。使用相同的种子编号和提示将产生相似的结束图像。

### **7. `--stop`**

使用`--stop`参数在流程中途完成作业。以较早的百分比停止作业会产生更模糊、更不详细的结果。

### **8. `--style`**

`--style <4a, 4b, or 4c>`在 Midjourney[模型版本](https://docs.midjourney.com/docs/models)4的版本之间
`--style <expressive, or cute>`切换 在 Niji[模型版本](https://docs.midjourney.com/docs/models)5的版本之间切换

### **9. `--stylize <number>`或`--s <number>`**

`--stylize <number>`，或`--s <number>` 参数会影响 Midjourney 的默认美学风格应用于 Jobs 的强度。

### **10. `--tile`**

用来生成可用作重复拼贴的图像，比如织物、壁纸、纹理等。

### **11. `--video`**

使用该`--video`参数创建正在生成的初始图像网格的短片。使用信封 ☉️ 表情符号对完成的工作做出反应，让 Midjourney Bot 将视频链接发送到您的直接消息。

> `--video`仅适用于图像网格，不适用于高档。
> `--video`适用于[模型版本](https://docs.midjourney.com/models) `1`、`2`、`3`、`test`和`testp`。

效果：

<div>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_VideoResult.gif">
</div>

### **12. `--uplight`**

![](/images/image-20230423175103467.png)

`--uplight`选择 U 按钮时使用替代的“轻型”升频器。结果更接近原始网格图像。放大后的图像细节更少，更平滑。

### **13. `--upbeta`**

![](/images/image-20230423175106275.png)

`--upbeta`选择 U 按钮时使用替代的 beta 升频器。结果更接近原始网格图像。放大后的图像添加的细节明显更少。

### **14. 默认参数(Model Version 4)**

|        |  --ar   |  --c  |     --q     |    --seed    | --stop |    --style    | --stylize |
| :----: | :-----: | :---: | :---------: | :----------: | :----: | :-----------: | :-------: |
| 默认值 |   1:1   |   0   |      1      |     随机     |  100   |      4c       |    100    |
|  范围  | 1:2–2:1 | 0–100 | .25 .5 or 1 | 0–4294967295 | 10–100 | 4a, 4b, or 4c |  0–1000   |



### **15. 默认参数 (Model Version 5)**

|        | --ar |  --c  |     --q      |     --c      | --stop | --stylize |
| :----: | :--: | :---: | :----------: | :----------: | :----: | :-------: |
| 默认值 | 1:1  |   0   |      1       |     随机     |  100   |    100    |
|  范围  | any  | 0–100 | .25 .5, or 1 | 0–4294967295 | 10–100 |  0–1000   |

- 大于 2:1 的宽高比是实验性的，可能会产生不可预测的结果。（实测有时会有黑边。）

## 二、`--seed`详细说明

### 1. `--seed`说明

用过 Midjourney 的小伙伴会发现在发送提示词后，MJ 最开始的图像里会有一个非常模糊的噪点团 ，然后逐渐变得具体清晰，而这个噪点团的起点就是“Seed”。
MJ 依靠它来创建一个视觉噪音场，作为生成初始图像的起点。每个图像的种子值是为随机生成的，但可以用 --seed 参数指定。
在 **v4** 模型中使用相同的种子值和提示词将产生相同的图像结果，利用这点我们可以生成连贯一致的人物形象或者场景。

Seed是Midjourney图像生成的初始点，每个图像的种子值是随机生成的，但可以用Seed参数保持统一。使用相同的种子值和提示词将产生完全相同的图像结果，利用这但可以生成连贯的人物形象或场景。

a rabbit --seed 4100004954，只要seed值一致，那么无论生成几次，只要关键词相同结果都是相同的。

### 2. 获取 `--seed`

使用时要确保服务器开启了隐私设置。

如果是自己创建的服务器，那么需要在个人服务器上右键 - 隐私设置

![](/images/image-20230414155204499.png)

也可以全局设置，Discord - Preferences - 隐私与安全 - 允许服务器成员直接向您发起私聊。

但要注意，这个设置不适用于已经加入的服务器。

![](/images/image-20230414155237311.png)

![](/images/image-20230414155338981.png)

### 3. 使用示例

首先输入描述，然后为描述指定`seed`，`seed`可以随机填，比如我这里写12345，那么生成的四宫格整张图的`seed`值就是`12345`

![](/images/image-20230423181147356.png)

比如我选择了图2作为接下来要持续生成的图，点击`U2`

![](/images/image-20230423182136592.png)

然后获取seed值

![](/images/image-20230424102129976.png)

图像上右键 - 选择envelope

![](/images/image-20230516103503514.png)

如果没有，点击添加反应 - 显示更多 - 搜索envelope并添加

![](/images/image-20230516103548000.png)

然后点击私信 - midjourney Bot，就可以查看了。

![](/images/image-20230516103830110.png)

拿到seed值后，根据上面的关键词，尽量不要变动，然后进一步描述场景、细节等

![](/images/image-20230424102243529.png)

就会发现生成的图片是基于上面的图片去变动的（虽然有时候变动的也挺一言难尽）。

![](/images/image-20230424102616997.png)

**seed值相同，描述值相同，生成的结果也相同。**

## 三、`--chaos`（`--c`）详细说明

#### `--chaos`参数影响初始图像网格变化程度。高`--chaos`值将产生更多不寻常和意想不到的结果和组合。较低的`--chaos`值具有更可靠、可重复的结果。

> `--chaos`接受值 0–100。
> 默认`--chaos`值为 0。

低chaos可以使结果图像差别低，

高chaos可以使结果图像差别高。

![](/images/image-20230424112750753.png)

![](/images/image-20230424112851699.png)

## 四、 `--remix`详细说明

#### 使用remix模式更改提示、参数、模型版本或变体之间的纵横比。Remix 将采用您的起始图像的一般构图，并将其用作新工作的一部分。重新混合可以帮助改变图像的设置或照明、发展主题或实现棘手的构图。

`Remix`是一项实验性功能，可能会随时更改或删除。

使用`/prefer remix`命令开启或关闭

![](/images/image-20230424120625511.png)

然后在生成图像后，点击`Make Variations`，会弹出`Remix`框。（若remix开启成功，按钮将变成绿色）

![](/images/image-20230424120842336.png)

我将 chinese girl 描述词变为了 cat，点击提交

![](/images/image-20230424120926944.png)

新生成的图片就会以原图为基础，女人变成了喵头。（看起来确实一言难尽吖。。。！！）

![](/images/image-20230424121226422.png)

## 五、`upscaler`详细说明

### 1. 简略说明

#### Midjourney 首先为每个作业生成一个低分辨率图像选项网格。您可以在任何网格图像上使用 Midjourney upscaler 来增加尺寸并添加更多细节。有多种可用于放大图像的放大模型。

<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">每个图像网格下方的按钮</font><font style="vertical-align: inherit;">用于放大所选图像。</font></font><span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">U1</code></span> <span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">U2</code></span> <span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">U3</code></span> <span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">U4</code></span><font style="vertical-align: inherit;"></font></p>

```
--uplight`, `--upbeta` or `--upanime
```

点击了某个图像放大器之后，会有一排按钮

![](/images/image-20230424113700430.png)

这就是升频器。

### 2. 对应尺寸和大小

*所有尺寸均适用于 1:1 宽高比的正方形。*

|      模型版本       |  起始网格大小   | V4 default Upscaler | Detail Upscale  |  Light Upscale   |  Beta Upscale   |  Anime Upscale  | Max Upscale** |
| :-----------------: | :-------------: | :-----------------: | :-------------: | :--------------: | :-------------: | :-------------: | :-----------: |
| **默认模型** **v4** |  **512 x 512**  |  **1024 x 1024** *  | **1024 x 1024** | 1 **024 × 1024** | **2048 x 2048** | **1024 x 1024** |       -       |
|       **v5**        | **1024 x 1024** |          -          |        -        |        -         |        -        |        -        |       -       |
|        v1–v3        |     256×256     |          -          |  1024 x 1024*   |   1024 x 1024    |   1024 x 1024   |   1024 x 1024   |  1664 x 1664  |
|        niji         |    512 x 512    |     1024 x 1024     |   1024 x 1024   |   1024 x 1024    |   2048 x 2048   |   1024 x 1024   |       -       |
|       niji 5        |   1024 x 1024   |          -          |        -        |        -         |        -        |        -        |       -       |
|     test/testp      |    512 x 512    |          -          |        -        |        -         |   2048 x 2048   |  1024 x 1024*   |       -       |
|         hd          |    512 x 512    |          -          |  1536 x 1536*   |   1536 x 1536    |   2048 x 2048   |        -        |  1024 x 1024  |

> `*`= 每个 Midjourney 版本模型的默认升频器。
> `**`Max Upscale 是一种较旧的资源密集型升频器，仅在用户处于快速模式时可用。

升频器分为Regular (Default) Upscaler、Light Upscaler、Detailed Upscaler、Beta Upscaler、Anime Upscaler、Remaster。切换方式就在每个图片生成后，下方的一排按钮

![](/images/image-20230424114321114.png)

官方文档对每种升频器的介绍非常详细，这里就不多说了。

> Midjourney Model 5
>
> The newest [Midjourney Model Version 5](https://docs.midjourney.com/models) (and Niji 5) produces high-resolution 1024 x1024 px image grids without needing an additional step to upscale each mage. When using Midjourney Model Version 5, the `U1` `U2` `U3` `U4` buttons under each image grid will separate the selected image from the initial image grid.

人话：v5模型和niji模型的默认升频器包含了放大图像的作用，因为初始生成的图像就已经是1024*1024了，所以点击`u1`、`u2`、`u3`、`u4`就是将每个图片单独提取出来而已。而其他版本的模型点击`u1`、`u2`、`u3`、`u4`是有放大并添加细节的作用的。

## 六、`--tile`详细说明        

用来生成可用作重复拼贴的图像，比如织物、壁纸、纹理等。

> `--tile`适用于[模型](https://docs.midjourney.com/models) 版本`1`、`2`、`3`和`5`。且仅仅生成一个瓦片。
>
> 若想根据该瓦片生成完整壁纸/图案/纹理的话，可以[使用像这种无缝模式检查器](https://www.pycheung.com/checker/)这样的模式制作工具来查看拼贴重复。

使用方法示例：

![](/images/image-20230423180758894.png)

它生成的结果是一块一块的，单独一块被称为瓦片。瓦片重复需要自己去拼接。

## 七、`--stylize`

#### Midjourney Bot 经过训练可以生成有利于艺术色彩、构图和形式的图像。或参数影响该训练应用`--stylize`的`--s`强度。

#### **低`--stylize`生成的图像与提示非常匹配，但艺术性较差。**

#### **高`--stylize`创建的图像非常具有艺术性，但与提示的联系较少。**

`--stylize`的默认值为 100，并且在使用默认 [V4 模型] 时接受 0-1000 的整数值。

不同的[Midjourney 版本模型](https://docs.midjourney.com/models)具有不同的风格化范围。

|            |   v5   |   v4   |    v3     | test/testp | Niji  |
| :--------: | :----: | :----: | :-------: | :--------: | :---: |
| 程式化默认 |  100   |  100   |   2500    |    2500    | other |
| 程式化范围 | 0–1000 | 0–1000 | 625–60000 | 1250–5000  | other |

![](/images/image-20230424113107952.png)

![](/images/image-20230424113120666.png)

## 八、复合提示

比如hot dog（热狗）这种词，如果想分开表示hot 和 dog （一只很热的狗），那就需要复合提示。

复合提示使用`::`表示

### 比如：

```
hot dog
```

![](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Multi_hotdog.jpg)

````
hot:: dog
````

![](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Multi_hot-dog.jpg)

### 再比如：

```
cup cake illustration
```

![](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Multi_cupCakeIllustration.jpg)

*纸杯蛋糕插图* 被认为是一起制作纸杯蛋糕的插图图像。

```
cup:: cake illustration
```

![Midjourney Prompt 杯子的图像:: 蛋糕插图::](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Multi_cup-cakeIllustration.jpg)

*杯子* 与 *蛋糕插图* 分开考虑，制作杯子中的蛋糕图像。

```
cup:: cake:: illustration
```

![](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Multi_cup-cake-illustration.jpg)

*杯子* 、 *蛋糕* 和 *插图* 被分开考虑，制作一个杯子里的蛋糕，带有花朵和蝴蝶等常见的插图元素。

### 权重
复合提示可以赋予权重
如`hot::2 dog`，意思是hot的重要性比dog大两倍，又可以写成`hot::2 dog::1`
+ v4可以接受小数做权重，v1-3只能接受整数，默认为1。
+ 可以接受负数权重，但所有权重的总和必须为正数。
+ 负数权重可以用来去掉某些元素
## 九、排列提升

可以同时处理多个作业，这个我暂时用不到，所以略过了。

简单的用法就是用大括号`{}`分割选项列表，然后快速创建多个作业，最多可以同时创建40个。

### 比如：
`/imagine prompt` `a {red, green, yellow} bird`

创建并处理三个作业。

相当于：

```
`/imagine prompt` `a red bird`
`/imagine prompt` `a green bird`
`/imagine prompt` `a yellow bird
```

那这样子就方便去创建一些只有部分元素不同，而整体装饰或构图相同的作业。

+ 可以固定部分描述不变，参数也不变，部分描述变体。

  ```
  /imagine prompt` `a naturalist illustration of a {pineapple, blueberry, rambutan, banana} bird
  ```

+ 或者固定部分参数不变，描述不变，尺寸变化等等。

  ```
  /imagine prompt` `a naturalist illustration of a fruit salad bird --ar {3:2, 1:1, 2:3, 1:2}
  ```

+ 支持多层嵌套

  ```
  /imagine prompt` `A {sculpture, painting} of a {seagull {on a pier, on a beach}, poodle {on a sofa, in a truck}}.
  ```

在我看来，有些像remix，但remix会在原图基础上调整。而这个是直接生成新的。

### 转义

大括号内用逗号做分割，但如果描述包含逗号，可以使用转义字符

```
imagine prompt` `{red, pastel \, yellow} bird`
```

相当于

```
`/imagine prompt` `a red bird`
`/imagine prompt` `a pastel, yellow bird
```


---
title: 【ai繪畫】Midjourney 之 Prompts
date: 2023-04-09 11:55:04
tags: 
  - ai繪畫
  - Midjourney
categories: 繪畫
---

## 一、Prompts

<p style="color:#9AB0FC">prompts（提示）是 Midjourney Bot 解释以生成图像的短文本短语。Midjourney Bot 将提示中的单词和短语分解为更小的部分，称为标记，可以将其与其训练数据进行比较，然后用于生成图像。精心制作的提示可以帮助制作独特而令人兴奋的图像。</p>

### 1. 结构

提示可以像一个单词、短语、emoji一样简单，也可以使用更高级的提示。

更高级的提示可以包括一个或多个[图像 URL](https://docs.midjourney.com/image-prompts)、[多个文本短语](https://docs.midjourney.com/multi-prompts)以及一个或多个[参数](https://docs.midjourney.com/parameter-list)

![](/images/image-20230404120022600.png)

<div style="background-color:#79a70e;color:white;font-weight:800;padding:10px;width:fit-content;border-radius:10px;margin:0">image Prompts</div>

可以将图像 URL 添加到提示中以影响最终结果的样式和内容。

**图片 URL 始终位于提示的前面。**

<div style="background-color:#828ef5;color:white;font-weight:800;padding:10px;width:fit-content;border-radius:10px;margin:0">Text Prompt</div>

要生成的图像的文本描述。

<div style="background-color:#d97706;color:white;font-weight:800;padding:10px;width:fit-content;border-radius:10px;margin:0">Parameters</div>

Parameters位于提示的末尾，它可以改变图像的生成方式。

用--表示。

可以更改宽高比、模型、放大器等等。

### 2. 需要注意的提示点

#### 长度

非常短的提示将在很大程度上依赖于 Midjourney 的默认样式。

更具描述性的提示更适合独特的外观。

#### 语法

不是越长越好，要提取重点词，专注于要创建的主要概念。因为midjourney不是像人类那样理解语法。

要用更精准的词汇，比如：大，是怎么样的大？

big, try gigantic, enormous, immense

#### 去掉不需要的内容

[`--no`](https://docs.midjourney.com/multi-prompts)参数加上的东西可以告诉midjourney，你不想在图像上出现什么

比如不想要蛋糕，可以加上`--no cake`

（唔、挺惊悚的。。生日没有蛋糕的怒气值叠满了！）

![](/images/image-20230404122021297.png)

#### 考虑哪些细节很重要

**尽量把很重要的任何背景或细节搞清楚：**

- **主题：**人、动物、人物、地点、物体等。
- **媒介：**照片、繪畫、插图、雕塑、涂鸦、挂毯等。
- **环境：**室内、室外、月球上、纳尼亚、水下、翡翠城等。
- **照明：**柔和、环境、阴天、霓虹灯、工作室灯等
- **颜色：**充满活力、柔和、明亮、单色、彩色、黑白、柔和等。
- **情绪：**稳重、平静、喧闹、精力充沛等。
- **构图：**肖像、头像、特写、鸟瞰图等。

**把官网的英文摘下来作参考：**

- **Subject:** person, animal, character, location, object, etc.
- **Medium:** photo, painting, illustration, sculpture, doodle, tapestry, etc.
- **Environment:** indoors, outdoors, on the moon, in Narnia, underwater, the Emerald City, etc.
- **Lighting:** soft, ambient, overcast, neon, studio lights, etc
- **Color:** vibrant, muted, bright, monochromatic, colorful, black and white, pastel, etc.
- **Mood:** Sedate, calm, raucous, energetic, etc.
- **Composition:** Portrait, headshot, closeup, birds-eye view, etc.

### 使用具体数字

比如"three birds"就比"birds"更加精确

### 3. Exprore Prompting（探索提示）

#### 选择媒介

提示示例：`/imagine prompt` `<any art style> style cat`

常用的媒介类型：

<div
	style="display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;
    margin: 2.5em 0 1.625em 0;
    text-align:center;color: #ffffff;"
	data-tomark-pass="">
	<div data-tomark-pass="">
		<strong style="color: #ffffff;"data-tomark-pass="">Block Print<br />版画 </strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Blockprint_Cat.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Folk Art<br />民间艺术</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Folkart.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Cyanotype<br />蓝版</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Cyanotype.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Graffiti<br />涂鸦</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Graffiti.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Paint-by-Numbers<br />数字繪畫</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Paint-by-numbers.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Risograph<br />数字印刷</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Risograph.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Ukiyo-e<br />浮世绘</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_UkiyoE.jpg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Pencil Sketch<br />铅笔素描</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_Pencilsketch.jpg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Watercolor<br />水彩</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cat_watercolor.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Pixel Art<br />像素画</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_pixelArt.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Blacklight Painting<br />紫外线画</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_blacklight.jpeg"
			data-tomark-pass="" />
	</div>
	<div data-tomark-pass="">
		<strong data-tomark-pass="">Cross Stitch<br />十字绣</strong>
		<img
			src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_crossStitch.jpeg"
			data-tomark-pass="" />
	</div>
</div>

### 细化

更精确的单词和短语将有助于创建具有完全正确外观和感觉的图像。

提示示例：`/imagine prompt` `<style> sketch of a cat`

<div class="gridsmall" data-tomark-pass="" style="display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;color: #ffffff;
    margin: 2.5em 0 1.625em 0;text-align: center;">
 <div data-tomark-pass="">
<strong data-tomark-pass="">Life Drawing<br />写生</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_lifeDrawingSketch.jpeg"data-tomark-pass="" data-lightbox="box">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Continuous Line<br />连续线</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_continuousLine.jpeg" data-tomark-pass="" data-lightbox="box">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Loose Gestural<br />随性手势画</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_looseGesturalSketch.jpeg" data-tomark-pass="" data-lightbox="box">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Blind Contour<br />草图速写</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_blindContour.jpeg" data-tomark-pass="" data-lightbox="box">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Value Study<br />明暗对比研究</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_valueStudy.jpeg" data-tomark-pass="" data-lightbox="box">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Charcoal Sketch<br />炭笔素描</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_Charcoal.jpeg" data-tomark-pass="" data-lightbox="box">
</div>
</div>



### 时间旅行

不同的时代有不同的视觉风格。

提示示例：`/imagine prompt` `<decade> cat illustration`

<div class="gridsmall" style="display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;text-align: center;
    margin: 2.5em 0 1.625em 0;color: #ffffff;
}" data-tomark-pass="">
<div data-tomark-pass="">
<strong data-tomark-pass="">1700s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1750.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1800s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1800.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1900s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1900.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1910s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1910.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1920s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1920.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1930s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1930.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1940s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1940.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1950s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1950.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1960s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1960.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1970s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1970.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1980s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1980.jpeg" data-lightbox="box">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass="">1990s</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_cat_1990.jpeg" data-lightbox="box">
</div>
</div>

### 表情

提示示例：`/imagine prompt` `<emotion> cat`

<div class="gridsmall" style="display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;color: #ffffff;text-align:center;
    margin: 2.5em 0 1.625em 0;" data-tomark-pass="">
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Determined<br />坚定</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Determined_cat.jpeg" data-tomark-pass="">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Happy<br />快乐</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Happy_Cat.jpeg" data-tomark-pass="">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Sleepy<br />困</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Sleepy_Cat.jpeg" data-tomark-pass="">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Angry<br />生气</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Angry_Cat.jpeg" data-tomark-pass="">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Shy<br />害羞</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Shy_cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Embarassed<br />尴尬</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Embarassed_Cat.jpeg">
</div>
</div>

### 使颜色更加丰富

提示示例：`/imagine prompt` `<color word> colored cat`

<div class="gridsmall" style="color: #ffffff;    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;text-align:center;
    margin: 2.5em 0 1.625em 0;" data-tomark-pass="">
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Millennial Pink<br />千禧粉</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_MillenialPink_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Acid Green<br />酸性绿</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Acid_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Desaturated<br />无饱和度</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Desaturated_cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Canary Yellow<br />金丝雀黄</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Canary_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Peach<br />桃色</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Peach_Cat.jpeg">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Two Toned<br />双色调</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Twotoned_cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Pastel<br />粉彩</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Pastel_cat.jpeg">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Mauve<br />淡紫色</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Mauve_Cat.jpeg">
</div>
 <div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Ebony<br />乌木色</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Ebony_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Neutral<br />中性色</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Neutral_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Day Glo<br />荧光色</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Dayglow_Cat.jpeg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass=""><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Green Tinted<br />绿色调</font></font></strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Greentinted_Cat.jpeg">
</div>
</div>

### 环境

提示示例：`/imagine prompt` `<location> cat`

<div class="gridsmall" style="color: #ffffff;    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 0.875rem;
    margin: 2.5em 0 1.625em 0;text-align: center;" data-tomark-pass="">
<div data-tomark-pass="">
<strong data-tomark-pass="">Tundra<br />苔原</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Tundra_Cat.jpg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Salt Flat<br />盐滩</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Saltflat_Cat.jpg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Jungle<br />密林</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Jungle_Cat.jpg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Desert<br />沙漠</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Desert_Cat.jpg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Mountain<br />山脉</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Mountain_Cat.jpg">
</div>
<div data-tomark-pass="">
<strong data-tomark-pass="">Cloud Forest<br />云雾森林</strong>
<img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Cloudforest_Cat.jpg">
</div>
</div>

### 使用/blend命令混合图像

<span style="color:#9AB0FC;font-weight:900">该`/blend`命令允许您快速上传 2-5 张图像，然后读取每张图像的风格和元素，并将它们合并成一个新颖的新图像。</span>

+ `/blend`与使用多个[图像提示](https://docs.midjourney.com/image-prompts)相同`/imagine`，但界面经过优化以方便在移动设备上使用。

+ `/blend`最多可处理 5 张图像。要在提示中使用 5 个以上的图像，请使用<span style="color:#9AB0FC;font-weight:900">`/imagine prompts`</span>

+ `/blend`，<span style="color:#9AB0FC;font-weight:900">不适用于文本提示</span>。要同时使用文本和图像提示，请使用<span style="color:#9AB0FC;font-weight:900">`/imagine prompts`</span>

![](/images/image-20230404120022600.png)

### `/blend`选项

输入`/blend`命令后，系统会提示您上传两张照片。

![](/images/image-20230410190100608.png)

要添加更多图像，可点击 **增加4** 并选择`image3`、`image4`或`image5`。

![](/images/image-20230410190226854.png)

**该`/blend`命令可能需要比其他命令更长的时间才能启动，因为您的图像必须在 Midjourney Bot 可以处理您的请求之前上传。**

`dimensions`：可设置生成图片比例。混合图像的默认纵横比为 1:1，但您可以使用可选`dimensions`字段在方形纵横比square (1:1)、纵向纵横比portrait (2:3) 或横向纵横比landscape (3:2) 之间进行选择。

![](/images/image-20230410190355527.png)

[自定义后缀](https://docs.midjourney.com/v1/docs/settings-and-presets)被添加到提示的末尾`/blend`，就像任何其他`/imagine`提示一样。作为`/blend`命令的一部分指定的宽高比会覆盖自定义后缀中的宽高比。

### 混合技巧

为获得最佳效果，请上传与您想要的结果具有**相同宽高比**的图像。


































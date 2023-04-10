---
title: 【ai绘画】midjourney入门（一）
date: 2023-04-03 11:37:57
tags: midjourney
categories: 绘画
---

本来不想搞，最近忙着准备面试呢，想了想还是整理一下文档吧。
ai绘画出来一个我就搁colab部署一个，midjourney掏点儿小钱就能省去我部署的步骤真是太好了。
而且也不像stable diffusion那样需要自己搜集训练模型
我体验过的ai绘画中算是效果最好，上手难度最低的一个。

## 一、安装及入门

### 1. 注册discord
+ midjourney的使用是通过discord访问，所以需要注册discord。
+ 点击进入：[discord官网](https://discord.com/)
+ 科学上网
#### 简单介绍discord
Discord是一款免费的语音、视频和文字聊天应用程序，它允许用户在私人或公共服务器上创建和加入聊天室或频道。
Discord的功能包括语音和视频通话、实时文本聊天、文件共享、屏幕共享、在线状态、自定义表情符号和徽章、服务器和频道管理工具等。它还提供了丰富的API，允许开发者创建自定义的机器人和集成其他服务。
通过Discord，用户可以方便地与其他人交流和合作，无论是在游戏、工作还是社交方面。它已经成为了一个非常流行的工具，有着庞大的用户群体和活跃的社区。

### 2. 访问midjourney.com

访问Midjourney.com，点击join the Beta，或直接前往[Midjourney Discord](https://discord.gg/midjourney)。

![](/images/image-20230403114938818.png)

### 3. 寻找newbies频道

在discord左侧，有房间可选

![](/images/image-20230403115710176.png)

随便选择一个进入即可。

### 4、 使用 /imagine 命令

1. 从斜杠命令弹出窗口中键入`/imagine prompt:`或选择命令。`/imagine`

![](/images/image-20230403120334073.png)

该`/imagine`命令从简短的文本描述（称为Prompt）生成一个独特的图像。

2. 初次使用时，Midjourney Bot 将生成一个弹出窗口，要求您接受服务条款。在生成任何图像之前，您必须同意服务条款。

### 5、处理图像

### Midjourney Bot 需要大约一分钟的时间来生成四个选项。

生成图像会激活免费的 Midjourney 试用版。试用用户有部分的免费时间，在需要订阅付费之前可以完成大约 25 个工作。

使用`/info`命令检查你的快速剩余时间查看您的剩余试用时间。

### 6、图像生成的基础上，进一步的变化

### 初始图像网格生成完成后，会出现两行按钮：

![](/images/image-20230403120853420.png)

```
U1` `U2` `U3` `U4
```

U 按钮[放大](https://docs.midjourney.com/upscalers)图像，生成所选图像的更大版本并添加更多细节。

```
V1` `V2` `V3` `V4
```

V 按钮创建所选网格图像的细微变化。创建变体会生成与所选图像的整体风格和构图相似的新图像网格。

```
🔄
```

🔄（重新滚动）重新运行作业。在这种情况下，它将重新运行原始提示，生成新的图像网格。

### 7. 评价图像

### 使用放大图像后，将出现一组新选项：

![](/images/image-20230403121020222.png)

```
🪄 Make Variations` 
`🔍 Light Upscale Redo`
`↗️Web
```

**Make Variations：**创建放大图像的变体并生成包含四个选项的新网格。

**Beta/Light Upscale Redo：**使用不同的升级器模型重做升级[。](https://docs.midjourney.com/upscalers)

**↗️Web**：在[Midjourney.com](https://www.midjourney.com/home/) 上打开图库中的图像

### 8.  保存图像

单击图像以全尺寸打开它，然后右键单击并选择`Save image`。

在手机上，长按图片，然后点击右上角的下载图标。

所有图片均可立即在[midjourney.com/app](https://www.midjourney.com/app/)
`Sign In with Discord`上查看。

### 9. 订阅服务

试用用户有试用时长，若不生成图片不会消耗，但也不会更新。要制作更多图像，请使用`/subscribe`在任何`newbies`频道中的命令生成指向中途帐户页面的个人链接。

![](/images/image-20230403121533718.png)

![](/images/image-20230403121603281.png)

**不要与他人分享此个人链接。**

请访问[订阅服务](https://docs.midjourney.com/plans)，获取有关价格和更多信息。

## 二、创建自己的个人服务器

在公共频道很多人都在生成图片，要翻找自己生成的图片很麻烦。

可以在discord创建自己的服务器并部署midjourney机器人，在自己的频道下使用midjourney。

**注：虽然在自己的个人服务器可以只展示自己的图片，但实际上生成的图片还是在midjourney公共区可以访问到。**

只有付费计划pro plan可以避免这点。

### 1. 创建服务器

点击+号 ---> 亲自创建 ---> 仅供我和我的朋友使用 ---> 创建

![](/images/image-20230403122133776.png)

![](/images/image-20230403122249556.png)

![](/images/image-20230403122304271.png)

![](/images/image-20230403122315091.png)

### 2. 进入频道，输入命令即可使用

![](/images/image-20230403122348895.png)



入门部分就到这里，之后我会用新文章整理关键字及命令部分。
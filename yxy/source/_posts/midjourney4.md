---
title: 【ai绘画】Midjourney 之 Param（参数）
date: 2023-04-10 19:06:10
tags: Midjourney
categories: 绘画
---

待补

关于 `--seed`

使用时要确保服务器开启了隐私设置。

如果是自己创建的服务器，那么需要在个人服务器上右键 - 隐私设置

![](/images/image-20230414155204499.png)

也可以全局设置，Discord - Preferences - 隐私与安全 - 允许服务器成员直接向您发起私聊。

但要注意，这个设置不适用于已经加入的服务器。

![](/images/image-20230414155237311.png)

![](/images/image-20230414155338981.png)

用过 Midjourney 的小伙伴会发现在发送提示词后，MJ 最开始的图像里会有一个非常模糊的噪点团 ，然后逐渐变得具体清晰，而这个噪点团的起点就是“Seed”，MJ 依靠它来创建一个视觉噪音场，作为生成初始图像的起点。每个图像的种子值是为随机生成的，但可以用 --seed 参数指定。在 v4 模型中使用相同的种子值和提示词将产生相同的图像结果，利用这点我们可以生成连贯一致的人物形象或者场景。
Seed是Midjourney图像生成的初始点，每个图像的种子值是随机生成的，但可以用Seed参数保持统一。使用相同的种子值和提示词将产生完全相同的图像结果，利用这但可以生成连贯的人物形象或场景。



a rabbit --seed 4100004954，只要seed值一致，那么无论生成几次，只要关键词相同结果都是相同的。

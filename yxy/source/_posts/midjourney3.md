---
title: 【ai繪畫】Midjourney 之 Commands（命令概述）
date: 2023-04-10 19:06:10
tags: 
  - ai繪畫
  - Midjourney
categories: 繪畫
---

## 一、命令简介

1. `/ask`：提问并获得答案。
2. `/blend`：轻松混合两个图像。
3. `/daily_theme`：切换是否接收 #daily-theme 频道更新的通知提醒。
4. `/docs`：在官方 Midjourney Discord 服务器中使用，快速生成本用户指南中涉及的主题链接！
5. `/faq`：在官方 Midjourney Discord 服务器中使用，快速生成常见提示工艺频道的常见问题解答链接。
6. `/fast`：切换到快速生成模式。无需排队，需要订阅Standard Plan或Pro Plan（可以使用快速生成模式的时间不同）。
7. `/help`：显示有关 Midjourney Bot 的有用基本信息和提示。
8. `/imagine`：使用提示生成图像。
9. `/info`：查看有关您的帐户以及任何排队或运行的作业的信息。
10. `/stealth`：对于 Pro 计划订阅者：切换到隐形模式。
11. `/public`：对于 Pro 计划订阅者：切换到公共模式m。
12. `/subscribe`：生成用户帐户页面的个人链接。
13. `/settings`：查看并调整 Midjourney Bot 的设置。
14. `/prefer option`：创建或管理自定义选项。
15. `/prefer option list`：查看您当前的自定义选项。
16. `/prefer suffix`：指定要添加到每个提示结尾的后缀。
17. `/show`：使用图像作业 ID 在 Discord 中重新生成作业。
18. `/relax`：切换到放松模式，对应`/fast`快速生成模式。
19. `/remix`：切换混合模式。使用方法：`/prefer remix`
20. `/settings`：设置midjourny默认工作方式。比如版本、快速模式、质量等

## 二、常用命令的详解

<span style="font-weight:900; color:#f05ef5; background: #ffff4f">**以上命令部分需要在官方 Midjourney Discord 服务器中使用，所以如果没有用，首先看看自己在不在midjourney的官方服务器。**</span>

2. `/blend`

   作用：融合2-5个图像的风格，生成新的图像。

   详见[Midjourney 之 Prompts](https://bbdcsg.fun/2023/04/09/midjourney2/) 使用`/blend`命令混合图像

4. `/docs`：查看文档![](/images/image-20230413175026367.png)

## Settings and Presets（设置与预设）



<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_settings.jpg" alt="显示 Midjourney 设置命令界面的图像" width="800px"></p>



<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">1️⃣ MJ Version 1</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">2️⃣ MJ Version 2</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">3️⃣ MJ Version 3</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">4️⃣ MJ Version 4</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">5️⃣ MJ Version 5</code></span>
</div>

**模型版本 5**<font style="vertical-align: inherit;"><font style="vertical-align: inherit;">仅适用于订阅了 Midjourney 的用户。</font></font>

<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🔥 Half Quality</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🔥 Base Quality</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🔥 High Quality (2x cost)</code></span>
</div> 

<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">设置</font><font style="vertical-align: inherit;">用于作业的</font></font><a href="/quality"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">质量参数。</font></font></a><font style="vertical-align: inherit;"></font><br><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
半质量 = </font></font><code data-backticks="1">--q .5</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，基本质量 = </font></font><code data-backticks="1">--q 1</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，高质量 = </font></font><code data-backticks="1">--q 2</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font><br>
<br data-tomark-pass=""></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Low</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Med</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style High</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Very High</code></span>
</div>  
mk
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">设置</font><font style="vertical-align: inherit;">用于作业的</font></font><a href="/stylize"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">风格化参数。</font></font></a><font style="vertical-align: inherit;"></font><br><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
风格低 = </font></font><code data-backticks="1">--s 50</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格中 = </font></font><code data-backticks="1">--s 100</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格高 = </font></font><code data-backticks="1">--s 250</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格非常高 = </font></font><code data-backticks="1">--s 750</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，</font></font><br>
<br data-tomark-pass=""></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🧍&zwj;♂️Public</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🕵️ Stealth</code></span>
</div>
<p><font style="vertical-align: inherit;"></font><a href="/stealth"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在公共模式和隐身模式</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">之间切换</font><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">对应于</font></font><code data-backticks="1">/public</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code data-backticks="1">/stealth</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令。</font></font><br>
<br></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🎛️ Remix</code></span>
</div>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">切换到</font></font><a href="/docs/remix"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">混音模式</font></font></a><br>
<br data-tomark-pass=""></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🐇 Fast</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🐢 Relax</code></span>
</div>
<p><font style="vertical-align: inherit;"></font><a href="/fast-relax"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在Fast 和 Relaxed 模式</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">之间切换</font><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">对应于</font></font><code data-backticks="1">/fast</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code data-backticks="1">/relax</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令。</font></font><br>
<br data-tomark-pass=""></p>

<section class="infoBox">
          <div class="title"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;color:#ffffff">设置说明
</font></font></div>
          <div class="content"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">添加到提示末尾的参数将覆盖使用 所做的选择</font></font><code data-backticks="1">/settings</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
</div></section>
<h2><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">自定义首选项</font></font></h2>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用 prefer 命令创建自定义选项，以自动将常用参数添加到提示末尾。</font></font><br>
<code data-backticks="1">/prefer auto_dm</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">完成的工作会自动发送到</font></font><span class="popover__title" data-glossary="Direct Message" data-glossaryid="f826e751-6db0-464b-a6c6-191ed45fe7fe"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">直接消息</font></font></span><br>
<code data-backticks="1">/prefer option</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">创建或管理自定义选项。</font></font><br>
<code data-backticks="1">/prefer option list</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">查看您当前的自定义选项。</font></font><br>
<code data-backticks="1">/prefer suffix</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">指定要添加到每个提示末尾的后缀。</font></font></p>
<h3><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">偏好选项</font></font></h3>
<p><code data-backticks="1">/prefer option set &lt;name&gt; &lt;value&gt;</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">创建可用于将多个参数快速添加到提示末尾的自定义参数。</font></font></p>
<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionSet.png" width="600px"></p>
<p><code data-backticks="1">/prefer option set</code> <code data-backticks="1">mine</code> <code data-backticks="1">--hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">创建一个名为“我的”的选项，转换为</font></font><code data-backticks="1">--hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font><br>
<br data-tomark-pass=""></p>
<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionSet_Used.jpg" width="600px"></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用</font></font><code data-backticks="1">/imagine prompt</code> <code data-backticks="1">vibrant California poppies --mine</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">, 被解释为</font></font><code data-backticks="1">/imagine prompt</code> <code data-backticks="1">vibrant California poppies --hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将“值”字段留空以删除选项。</font></font><br>
<br data-tomark-pass=""></p>
<p><code data-backticks="1">/prefer option list</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">列出使用创建的所有选项</font></font><code data-backticks="1">prefer option set.</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">  用户最多可以有 20 个自定义选项。</font></font></p>
<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionList.png" width="400px"></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要删除自定义选项，请使用</font></font><code data-backticks="1">/prefer option set</code> <code data-backticks="1">&lt;name to delete&gt;</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">值字段并将其留空。</font></font></p>

<h3><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">首选后缀</font></font></h3>
<p><code data-backticks="1">/prefer suffix</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在所有提示后自动附加指定的后缀。</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用不带值的命令进行复位。</font></font></strong></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令示例：</font></font><code data-backticks="1">/prefer suffix</code> <code data-backticks="1">--uplight --video</code></p>

<section class="infoBox">
          <div class="title"></div>
          <div class="content"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Only </font></font><a href="/parameter-list"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Parameters</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以与 一起使用</font></font><code data-backticks="1">/prefer suffix</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，</font></font><br>
<code data-backticks="1">prefer suffix --no orange</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">is accepted</font></font><br>
<code data-backticks="1">prefer suffix orange::-1</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">不接受</font></font></p>
</div></section>

<div class="glossary-popover-contents"><div class="popover__content hidden f826e751-6db0-464b-a6c6-191ed45fe7fe"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">订阅者可以在 Discord 的直接消息中与 Midjourney Bot 进行一对一的工作，而不是通过公共频道。</font><font style="vertical-align: inherit;">在您的直接消息中制作的图像仍受内容和审核规则的约束，并且将在您的 Midjourney 网站画廊中可见。</font></font></p></div></div>
                    
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">设置</font><font style="vertical-align: inherit;">用于作业的</font></font><a href="/quality"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">质量参数。</font></font></a><font style="vertical-align: inherit;"></font><br><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
半质量 = </font></font><code data-backticks="1">--q .5</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，基本质量 = </font></font><code data-backticks="1">--q 1</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，高质量 = </font></font><code data-backticks="1">--q 2</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font><br>
<br data-tomark-pass=""></p>


<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Low</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Med</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style High</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🖌️ Style Very High</code></span>
</div>  
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">设置</font><font style="vertical-align: inherit;">用于作业的</font></font><a href="/stylize"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">风格化参数。</font></font></a><font style="vertical-align: inherit;"></font><br><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">
风格低 = </font></font><code data-backticks="1">--s 50</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格中 = </font></font><code data-backticks="1">--s 100</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格高 = </font></font><code data-backticks="1">--s 250</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，风格非常高 = </font></font><code data-backticks="1">--s 750</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，</font></font><br>
<br data-tomark-pass=""></p>

<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🧍&zwj;♂️Public</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🕵️ Stealth</code></span>
</div>

<p><font style="vertical-align: inherit;"></font><a href="/stealth"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在公共模式和隐身模式</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">之间切换</font><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">对应于</font></font><code data-backticks="1">/public</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code data-backticks="1">/stealth</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令。</font></font><br>
<br></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🎛️ Remix</code></span>
</div>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">切换到</font></font><a href="/docs/remix"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">混音模式</font></font></a><br>
<br data-tomark-pass=""></p>
<div class="buttonrowlarge" data-tomark-pass="">
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🐇 Fast</code></span>
<span class="discordbtn" data-tomark-pass=""><code data-tomark-pass="">🐢 Relax</code></span>
</div>
<p><font style="vertical-align: inherit;"></font><a href="/fast-relax"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在Fast 和 Relaxed 模式</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">之间切换</font><font style="vertical-align: inherit;">。</font><font style="vertical-align: inherit;">对应于</font></font><code data-backticks="1">/fast</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">和</font></font><code data-backticks="1">/relax</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令。</font></font><br>
<br data-tomark-pass=""></p>
<section class="infoBox">
          <div class="title"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;color:#ffffff">设置说明
</font></font></div>
          <div class="content"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">添加到提示末尾的参数将覆盖使用 所做的选择</font></font><code data-backticks="1">/settings</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
</div></section>

<h2><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">自定义首选项</font></font></h2>

<p>使用 prefer 命令创建自定义选项，以自动将常用参数添加到提示末尾。<br>
<code>/prefer auto_dm</code>完成的工作会自动发送到option set &lt;name&gt; &lt;value&gt;</code>创建可用于将多个参数快速添加到提示末尾的自定义参数。</p>

<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionSet.png"></p>

<p><code data-backticks="1">/prefer option set</code> <code data-backticks="1">mine</code> <code data-backticks="1">--hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">创建一个名为“我的”的选项，转换为</font></font><code data-backticks="1">--hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">.</font></font><br>
<br data-tomark-pass=""></p>

<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionSet_Used.jpg" width="600px"></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用</font></font><code data-backticks="1">/imagine prompt</code> <code data-backticks="1">vibrant California poppies --mine</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">, 被解释为</font></font><code data-backticks="1">/imagine prompt</code> <code data-backticks="1">vibrant California poppies --hd --ar 7:4</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">。</font></font></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">将“值”字段留空以删除选项。</font></font><br>
<br data-tomark-pass=""></p>

<p><code data-backticks="1">/prefer option list</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">列出使用创建的所有选项</font></font><code data-backticks="1">prefer option set.</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">  用户最多可以有 20 个自定义选项。</font></font></p>
<p><img src="https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/PreferOptionList.png" width="400px"></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">要删除自定义选项，请使用</font></font><code data-backticks="1">/prefer option set</code> <code data-backticks="1">&lt;name to delete&gt;</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">值字段并将其留空。</font></font></p>

<h3><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">首选后缀</font></font></h3>
<p><code data-backticks="1">/prefer suffix</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">在所有提示后自动附加指定的后缀。</font></font><strong><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">使用不带值的命令进行复位。</font></font></strong></p>
<p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">命令示例：</font></font><code data-backticks="1">/prefer suffix</code> <code data-backticks="1">--uplight --video</code></p>

<section class="infoBox">
          <div class="title"></div>
          <div class="content"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Only </font></font><a href="/parameter-list"><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">Parameters</font></font></a><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">可以与 一起使用</font></font><code data-backticks="1">/prefer suffix</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">，</font></font><br>
<code data-backticks="1">prefer suffix --no orange</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">is accepted</font></font><br>
<code data-backticks="1">prefer suffix orange::-1</code><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">不接受</font></font></p>
</div></section>
<div class="glossary-popover-contents"><div class="popover__content hidden f826e751-6db0-464b-a6c6-191ed45fe7fe"><p><font style="vertical-align: inherit;"><font style="vertical-align: inherit;">订阅者可以在 Discord 的直接消息中与 Midjourney Bot 进行一对一的工作，而不是通过公共频道。</font><font style="vertical-align: inherit;">在您的直接消息中制作的图像仍受内容和审核规则的约束，并且将在您的 Midjourney 网站画廊中可见。</font></font></p></div></div>
                
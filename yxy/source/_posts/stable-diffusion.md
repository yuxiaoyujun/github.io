---
title: 【stable_diffusion】使用阿里雲遠程創建stable diffusion模型
date: 2024-05-06 15:36:24
tags: 
  - ai繪畫
categories:
  - [程序员的自我修养]
  - [繪畫]
---

在同時學習粵拼所以打字會用粵語拼音輸入（jap6）法（faat3），有些文字唔識打，又或（waak6）者（ze2）唔係好明咩意思嘅詞語，之後會改（tabunn。。）。

## stable diffusion 是什麼

Stable Diffusion 3 是最先進的圖像模型，搭載了最新的文本圖像轉換技術，大幅提升了多主題提示、圖像質量和拼寫輸入的能力。

stable diffusion的原理就係利用緊一個大model同一個小model（微調model）共同生成一個圖片

## 使用阿里雲部署stable diffusion嘅步驟

### 1.  開通函數計算FC

打開阿里雲 - 函數計算FC - 立即開通 - 立即購買

![](/images/image-20240506160439777.png)

![](/images/image-20240506160543609.png)

![](/images/image-20240506160619111.png)

購買完成後，進入控制台

![](/images/image-20240506165704715.png)

### 2. 創建角色

進入控制台後，點擊左側應用，提示沒有AliyunFcDefaultRole角色，點擊創建，同意授權。

![](/images/image-20240506165936955.png)

![](/images/image-20240506170041720.png)

### 3.  創建應用

為了方便新手使用雲計算平台，所以阿里雲有創建模版應用的選項，此處使用模版應用，當然都可以通過github等將代碼部署過來的方式使用。

按照以下方式創建stable diffusion應用。

![](/images/image-20240506170847144.png)

點擊**“立即創建”**，跳轉到項目配置，配置角色名為剛才創建的角色，完成後，點擊**“創建應用”**。

![](/images/image-20240506171003658.png)

勾選同意，同意並繼續部署，等待部署成功。

![](/images/image-20240506171142441.png)

部署成功後，可以查看部署日志。

![](/images/image-20240506171402117.png)

### 3. 查看日誌

在應用詳情中，可以查看日誌

![](/images/image-20240506171849889.png)

在這個地方點擊查看日志，下方可以查看配置，即阿里雲為了應用分配的規格參數。

![](/images/image-20240506171958565.png)

*注：阿里雲嘅函數計算係按量計費，詳見[函数计算](https://help.aliyun.com/zh/fc/product-overview/billing-overview?spm=5176.28100096.0.0.123f8URl8URlDb)*

### 3.  開通NAS文件系統

此時應用已部署，但沒有存儲模型的硬盤，NAS文件系統就相當於存儲模型的硬盤。

在應用頁面點擊**“初始化模型管理”**

![](/images/image-20240506172451888.png)

掛載硬盤NAS，若是新用戶可以領取免費試用。

![](/images/image-20240506172805926.png)

點擊立即開通

![](/images/image-20240506172843424.png)

成功後，訪問web UI域名。

![](/images/image-20240506172921409.png)

### 4. 正式使用

在該頁面，可正常使用stable diffusion

![](/images/image-20240506173018997.png)

![](/images/image-20240506173430466.png)

t步驟（bou6 zaau6）

部署（bou6 cyu5）

函數計算（haam4 sou3 gai3 syun3）

立即開通（laap6 zik1 hoi1 tung1 ）

計費（gai3 fai3）

購買（gau3 maai）

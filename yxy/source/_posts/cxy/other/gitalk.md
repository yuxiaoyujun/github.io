---
title: '在github pages中集成Gitalk功能'
date: 2019-12-11 18:48:34
tags: 实用开源代码
categories:
  - 程序员的自我修养
---
  <meta name="referrer" content="no-referrer">




## Gitalk简介

Gitalk 是一个基于 Github Issue 和 Preact 开发的评论组件，支持 MarkDown 语法。

官方地址

https://gitalk.github.io/

特性

+ 使用 github 帐户进行身份验证

+ 无服务器，所有评论将存储为github问题

+ 个人和组织github项目均可用于存储评论

+ 本地化，支持多种语言[en，zh-CN，zh-TW，es-ES，fr，ru，de]

+ 类似于Facebook的无干扰模式（可以通过distractionFreeMode选件启用）

+ 热键提交评论（cmd | ctrl + Enter）

安装方法有两种

- links

```javascript
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
  <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>

  <!-- or -->

  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```

- npm install

```javascript
npm i --save gitalk
import 'gitalk/dist/gitalk.css'
import Gitalk from 'gitalk'
```

## 用法

1. 首先创建一个公共的 github 存储库来存储评论。（现有的也ok，反正只是用到issue不会影响仓库代码。）

2. 创建一个**GitHub 应用程序**，创建方法请[单击此处注册](https://github.com/settings/applications/new)一个新应用程序。

 **注意：** 必须在字段中指定网站域 url `Authorization callback URL`（回调的地址），都写你的个人网站地址就可以了。
![](https://upload-images.jianshu.io/upload_images/20892169-82c3c3b4e810dd11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
创建成功后，跳转到此
![](https://upload-images.jianshu.io/upload_images/20892169-61c661153203e3eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
记住clientID和clientSecret，是之后要配置的。

3. 选择如何应用到页面，如下所示：

### 方法：

将容器添加到您的页面：

```html
<div id="gitalk-container"></div>
```

然后使用下面的Javascript代码生成gitalk插件：

```js
const gitalk = new Gitalk({
  clientID: 'GitHub Application Client ID',
  clientSecret: 'GitHub Application Client Secret',
  repo: 'GitHub repo',      // The repository of store comments,
  owner: 'GitHub repo owner',
  admin: ['GitHub repo owner and collaborators, only these guys can initialize github issues'],
  id: location.pathname,      // Ensure uniqueness and length less than 50
  distractionFreeMode: false  // Facebook-like distraction free mode
})

gitalk.render('gitalk-container')
```
其中clientID和clientSecret是上面生成的，填入你的就可以了。
## 选项

*   **客户端ID** `String`

    **必填**。GitHub 应用程序客户端 ID。

*   **客户秘密** `String`

    **必填**。GitHub 应用程序客户端机密。

*   **回购** `String`

    **必填**。GitHub 存储库。

*   **所有者** `String`

    **必填**。GitHub 存储库所有者。可以是个人用户或组织。

*   **行政** `Array`

    **必填**。GitHub 存储库所有者和协作者。（对该存储库具有写入权限的用户）

*   **ID** `String`

    默认值：`location.href`.

    页面的唯一标识。长度必须小于 50。

*   **数字** `Number`

    默认值：`-1`.

    页面的issue ID，如果`number`没有定义该属性，则使用.issue定位issue `id`。

*   **标签** `Array`

    默认值：`['Gitalk']`.

    GitHub 问题标签。

*   **标题** `String`

    默认值：`document.title`.

    GitHub 问题标题。

*   **身体** `String`

    默认值：`location.href + header.meta[description]`.

    GitHub 问题正文。

*   **语言** `String`

    默认值：`navigator.language || navigator.userLanguage`.

    本地化语言键，支持[ `en`, `zh-CN`, `zh-TW`, `es-ES`, `fr`, `ru`, `de`, `pl`, `ko`, `fa`, `ja`]。

*   **每页** `Number`

    默认值：`10`.

    分页大小，最大 100。

*   **无干扰模式** `Boolean`

    默认值：假。

    类似 Facebook 的无干扰模式。

*   **pagerDirection** `String`

    默认值：“最后”

    评论排序方向，可选值为`last`和`first`。

*   **手动创建问题** `Boolean`

    默认值：`false`.

    默认情况下，当登录用户属于用户时，Gitalk 会自动为您的每个页面创建一个相应的 github 问题`admin`。您可以通过将此选项设置为 来手动创建它`true`。

*   **代理人** `String`

    默认值：`https://cors-anywhere.azm.workers.dev/https://github.com/login/oauth/access_token`.

    GitHub oauth 请求 CORS 的反向代理。[为什么需要这个？](https://github.com/isaacs/github/issues/330)

*   **翻转移动选项** `Object`

    默认：

    ```source-js
      {
        staggerDelayBy: 150,
        appearAnimation: 'accordionVertical',
        enterAnimation: 'accordionVertical',
        leaveAnimation: 'accordionVertical',
      }
    ```

    评论列表动画。[参考](https://github.com/joshwcomeau/react-flip-move/blob/master/documentation/enter_leave_animations.md)

*   **启用热键** `Boolean`

    默认值：`true`.

    启用热键 (cmd|ctrl + enter) 提交评论。

## 效果演示
[点击访问](https://bbdcsg.fun/2019/12/11/cxy/other/gitalk/)
![](https://upload-images.jianshu.io/upload_images/20892169-fa80cc39adc7320d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
title: 【nodejs】Nodemon的使用
date: 2019-06-01 11:02:45
tags: 
  - node
categories: 程序员的自我修养
---

## 什么是 Nodemon？

Nodemon 是一个用于监视 Node.js 应用程序文件的工具，并在文件发生更改时自动重新启动应用程序，以加速开发过程。它是一个非常方便的开发工具，可以帮助你在代码编辑过程中快速查看修改的效果，无需手动停止和重新启动应用程序。

## 安装 Nodemon

你可以使用 npm 或 yarn 来全局安装 Nodemon：

```bash
npm install -g nodemon
# 或者
yarn global add nodemon
```

## 使用 Nodemon

一旦安装了 Nodemon，你可以通过在终端中运行以下命令来启动你的 Node.js 应用程序：

```bash
nodemon your-app.js
```

在这个命令中，`your-app.js` 是你的 Node.js 应用程序的入口文件。Nodemon 将会监视这个文件以及其所依赖的文件。

## 高级用法

### 自定义配置

你可以创建一个 `nodemon.json` 文件来配置 Nodemon 的行为。在这个文件中，你可以指定需要监视的文件、忽略的文件、以及其他配置选项。以下是一个示例 `nodemon.json` 文件：

```bash
{
  "watch": ["src", "config"],
  "ext": "js json",
  "ignore": ["src/static"]
}
```

在这个示例中，Nodemon 将监视 `src` 和 `config` 目录中的文件，只有当 `.js` 和 `.json` 文件发生更改时才会重新启动应用程序，同时忽略 `src/static` 目录。

### 使用命令

除了直接在命令行中运行 Nodemon，你还可以将 Nodemon 集成到 `package.json` 中的 `scripts` 中，以便使用自定义的启动命令。例如：

```bash
{
  "scripts": {
    "start": "nodemon your-app.js"
  }
}
```

然后，你可以通过运行 `npm start` 来启动应用程序，这将使用 Nodemon 自动监视文件。

### 更多选项

Nodemon 支持许多其他选项，可以根据需要进行配置，例如设置端口、调试选项等。你可以在官方文档中查找完整的选项列表和示例：[Nodemon 官方文档](https://github.com/remy/nodemon)

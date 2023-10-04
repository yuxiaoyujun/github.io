---
title: 组件库相关
date: 2023-03-30 19:01:18
tags: 
 - lv2
categories: 程序员的自我修养
---
## 一、思维整理
移动端的前端组件库可以包括许多组件，这里列出一些常见的组件：
按钮组件：用于触发操作或提交表单。
图标组件：用于显示图标。
输入框组件：包括文本输入框、数字输入框等。
列表组件：用于展示列表信息。
滚动组件：用于滚动显示大量内容。
轮播组件：用于轮播广告或图片。
弹窗组件：用于弹出提示或确认框。
标签组件：用于分类或标记信息。
搜索组件：用于搜索信息。
日期时间组件：用于选择日期时间。
下拉选择组件：用于选择下拉菜单中的选项。
滑块组件：用于选择范围或值。
表单组件：包括表单验证、提交等。
树形组件：用于显示层级结构。
下拉刷新：下拉时可以重新加载数据。
上拉加载更多：当data很多时分页适用。
头像+头像框组件：有些活动中会有不规则头像框，或者vip的特效等。
搜索文本框：带搜索按钮的文本款组件等。
再比如性能优化方面，防抖、节流等

jsbridge相关：调用相机、调用相册、分享等

组件库搭建：
展示方面：用react/vue + bootstrap4 做框架，codesandbox做代码演示，还有复制链接及效果，类似antd组件的网站。
然后使用npm包安装，引入使用。

比如说，我现在想搭一个npm包，里面包括按钮组件、搜索框组件等。
这里我就只写两个按钮组件、搜索框组件

## 二、npm包的开发示例
#### 1. 安装依赖
```css
npm install webpack webpack-cli vue-loader vue-template-compiler css-loader style-loader --save-dev
```
#### 2. 在项目根目录下创建一个 webpack.config.js 文件，配置如下：

在项目根目录下创建一个 webpack.config.js 文件，配置如下：

```js
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
  mode: 'production', // 打包模式
  entry: './src/index.js', // 入口文件
  output: { // 输出配置
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-button.js',
    library: 'MyButton',
    libraryTarget: 'umd'
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin()
  ]
};
```
其中，`entry` 字段指定了入口文件，`output` 字段指定了输出配置，这里生成了一个名为 `my-button.js` 的文件，`library` 指定了模块名称，`libraryTarget` 指定了模块的导出方式。
#### 3. 在 `src` 目录下创建 `index.js` 文件和 `Button.vue` 文件，分别编写代码如下：
```js
import Vue from 'vue';
import Button from './Button.vue';

const MyButton = {
  install: function(Vue) {
    Vue.component('MyButton', Button);
  }
};

Vue.use(MyButton);

export default MyButton;
```
Button.vue：

```html
<template>
  <button class="my-button" :style="{height: height, width: width}"><slot></slot></button>
</template>

<script>
export default {
  name: 'MyButton',
  props: {
    height: {
      type: String,
      default: '60px'
    },
    width: {
      type: String,
      default: '80%'
    }
  }
}
</script>

<style scoped>
.my-button {
  background-color: #f0ff00;
  border: none;
  color: white;
  font-size: 16px;
}
</style>

```
#### 4. 在命令行中运行以下命令打包：
```
npx webpack
```
### 5. 引用
将打包后的文件（即 my-button.js）上传到 npm 上，然后在需要使用该组件库的项目中，通过以下命令安装该组件库：
```js
npm install your-package-name
```
在项目中使用该组件库，可以像下面这样引入：

```js
import Vue from 'vue';
import MyButton from 'your-package-name';

Vue.use(MyButton);
```
之后就可以在Vue 组件中使用 MyButton 组件了，像这样：
```html
<template>
  <div>
    <my-button>Click Me</my-button>
  </div>
```

## 三、在原有组件基础上加入新的搜索文本框组件

思路：先将新的搜索文本框组件写好并打包成一个单独的模块。然后，可以在 my-button 组件中引入新的搜索文本框组件并进行相应的配置和使用。最后，将 my-button 和搜索文本框组件打包成一个组件库并发布到 npm 上。

具体的步骤如下：

#### 1. 编写搜索文本框组件
具体的编写就不说了，假设已经编辑好，命名为 my-input。
创建一个webpack.config.js文件，配置打包规则：

```js
const path = require('path');

module.exports = {
  mode: 'production', // 设置为生产环境模式
  entry: './src/index.js', // 入口文件路径
  output: {
    path: path.resolve(__dirname, 'dist'), // 输出目录路径
    filename: 'flow-components.min.js', // 输出文件名称
    library: 'flowComponents', // 打包生成的库的名称
    libraryTarget: 'umd', // 打包生成的库的目标环境
    umdNamedDefine: true // 是否需要命名 UMD 模块
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      },
      {
        test: /\.css$/,
        use: ['css-loader', 'vue-style-loader', 'style-loader']
      },
      {
        test: /\.(png|jpe?g|gif|svg)$/i,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: '[name].[ext]',
              outputPath: 'images/'
            }
          }
        ]
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.vue'] // 支持的文件扩展名
  }
};

```
这里使用了 vue-loader 处理 .vue 单文件组件，使用 css-loader、vue-style-loader 和 style-loader 处理样式文件，使用 file-loader 处理静态资源文件。设置了输出目录路径、输出文件名称、打包生成的库的名称、打包生成的库的目标环境等。

#### 2. 在 src 目录下创建一个 index.js 入口文件，用于导出组件：
```js
import Button from './components/Button.vue';
import Input from './components/Input.vue';

const components = {
  Button,
  Input
};

const install = function (Vue) {
  if (install.installed) return;
  Object.keys(components).forEach((name) => {
    Vue.component(name, components[name]);
  });
};

if (typeof window !== 'undefined' && window.Vue
```
#### 3. 引入
```js
npm install your-library-name --save
```
在项目中引用组件库中的组件，首先需要在代码中引入组件，比如：
```
import { Button, SearchInput } from 'your-library-name';
```
然后在代码中直接使用引入的组件即可：
```
<Button>Click me</Button>
<SearchInput placeholder="Search" />
```

## 三、搭建boostrap+vue的网页端

待补充
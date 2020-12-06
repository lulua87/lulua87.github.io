---
title: "webpack使用总结"
date: 2018-04-09 14:10:00
tags: ['webpack', '前端入门']
description: "本质上，[webpack](https://www.webpackjs.com/) 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。"

---

## 前言

本质上，[webpack](https://www.webpackjs.com/) 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

四个核心概念:

+ 入口(entry)
+ 输出(output)
+ loader
+ 插件(plugins)

---

## 一、为什么选择webpack

+ 它和browserify类似 但是它可以把你的应用拆分成多个文件。如果你的单页应用里有很多页面，用户只会下载当前访问页面的代码。当他们访问应用中的其他页面时，不再需要加载与之前页面重复的通用代码。
+ 它可以替代*gulp*和grunt 因为他可以构建打包css、预处理css、编译js和图片等。
+ 支持AMD和CommonJS，以及其他的模块系统(Angular, ES6)

## 四个核心概念介绍

### 入口(entry)

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。
每个依赖项随即被处理，最后输出到称之为 bundles 的文件中。

### 出口(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。
webpack配置文件：webpack.config.js

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

### loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

本质上，webpack loader 将所有类型的文件，转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

webpack.config.js

```js
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};
module.exports = config;
```

> “嘿，webpack 编译器，当你碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先使用 raw-loader 转换一下。”

### 插件(plugins)
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。

想要使用一个插件，你只需要 require() 它，然后把它添加到 plugins 数组中。多数插件可以通过选项(option)自定义。

```js
webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

const config = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```
 
### 模式

通过选择 开发环境 *development* 或 线上环境*production* 之中的一个，来设置 mode 参数，你可以启用相应模式下的 webpack 内置的优化

```js
module.exports = {
  mode: 'production'
};
```

> 或者从 命令行接口(Command Line Interface) CLI 参数中传递 : `webpack --mode=production`

## 入口

单个入口：

```js
const config = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};
```

分离 应用程序(app) 和 第三方库(vendor) 入口:

```js
webpack.config.js
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```

多页面应用程序 webpack.config.js:

```js
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
// 我们告诉 webpack 需要 3 个独立分离的依赖图
```

## 输出(Output)

多个入口起点:

```js
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}

// 写入到硬盘：./dist/app.js, ./dist/search.js
```

## 基本配置

webpack.config.js

```js
var path = require('path');

module.exports = {
  mode: 'development',
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js'
  }
};
```

## 模块(Modules)

什么是 webpack 模块对比 Node.js 模块，webpack _模块_能够以各种方式表达它们的依赖关系，几个例子如下：

+ ES2015 `import` 语句
+ CommonJS `require()` 语句
+ AMD `define `和 `require` 语句
+ css/sass/less 文件中的 `@import` 语句。
+ 样式(`url(...)`)或 HTML 文件(`<img src=...>`)中的图片链接(image url)

> webpack 1 需要特定的 `loader` 来转换 ES 2015 `import`，然而通过 webpack 2 可以开箱即用

## demo

[Webpack构建前端工程项目](http://jypblue.github.io/blog/2016/06/27/webpack-build-project/)
---
title: webpack中的Loader总结
cover: 'https://img.showydream.com/img/m2XSRQ-2019-07-04-01.png'
description: Webpack Q&A笔记，持续更新
keywords: Webpack，Loader，面经
tags: Webpack
categories:
  - Webpack
abbrlink: e6ad105
date: 2021-11-03 17:54:58
updated: 2021-11-05 14:00:00
---

## Loader介绍

webpack做的事情，仅仅是分析出各种模块的依赖关系，然后整理成资源列表，最终打包生成到指定的文件中。

上一节我们说过，在webpack内部，任何文件都是模块，不仅仅是`js`文件。但是在默认情况下，在遇到`require`和`import`的时候，webpack只支持对`js`和`json`文件的打包，像`css`、`scss`、`jpg`等这些类型的文件，webpack就无能为力了，这时候就用到了loader来对文件的内容进行解析

> loader的定义：loader用于对模块的源代码进行转换，在import或加载模块时预处理文件

在加载模块时，执行顺序如下：

<img src="https://img.showydream.com/img/TFRENB-image-20211104120757749.png" alt="image-20211104120757749" style="zoom:50%;" />

##### 当webpack碰到识别不了的文件时，就会从配置中找该文件的解析规则，来看个示例：

使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JavaScript。为此，首先安装相对应的 loader：

```bash
npm install --save-dev css-loader ts-loader
```

然后指示 webpack 对每个 `.css` 使用 [`css-loader`](https://webpack.docschina.org/loaders/css-loader)，以及对所有 `.ts` 文件使用 [`ts-loader`](https://github.com/TypeStrong/ts-loader)：

**webpack.config.js**

```javascript
module.exports = {
  module: {
    rules: [
      // 将所有TypeScript 转为 JavaScript，
      // 也就意味着我们使用ts来开发，最终转换成js运行在浏览器端
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```

[`module.rules`](https://webpack.docschina.org/configuration/module/#modulerules) 允许你在 webpack 配置中指定多个 loader。 这种方式是展示 loader 的一种简明方式，并且有助于使代码变得简洁和易于维护。同时让你对各个 loader 有个全局概览：

loader **从右到左**（或**从下到上**）地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，然后继续执行 css-loader，最后以 style-loader 为结束。

```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: 'style-loader' },
          // [css-loader](/loaders/css-loader)
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

## loader 特性

- loader 支持链式调用。链中的每个 loader 会将转换应用在已处理过的资源上。一组链式的 loader 将按照相反的顺序执行。链中的第一个 loader 将其结果（也就是应用过转换后的资源）传递给下一个 loader，依此类推。最后，链中的最后一个 loader，返回 webpack 所期望的 JavaScript。
- loader 可以是同步的，也可以是异步的。
- loader 运行在 Node.js 中，并且能够执行任何操作。
- loader 可以通过 `options` 对象配置（仍然支持使用 `query` 参数来设置选项，但是这种方式已被废弃）。
- 除了常见的通过 `package.json` 的 `main` 来将一个 npm 模块导出为 loader，还可以在 module.rules 中使用 `loader` 字段直接引用一个模块。
- 插件(plugin)可以为 loader 带来更多特性。
- loader 能够产生额外的任意文件。

可以通过 loader 的预处理函数，为 JavaScript 生态系统提供更多能力。用户现在可以更加灵活地引入细粒度逻辑，例如：压缩、打包、语言转译（或编译）

## 常见的Loader

### 文件

- [`val-loader`](https://webpack.docschina.org/loaders/val-loader) 将代码作为模块执行，并将其导出为 JS 代码
- [`ref-loader`](https://www.npmjs.com/package/ref-loader) 用于手动建立文件之间的依赖关系

### JSON

- [`cson-loader`](https://github.com/awnist/cson-loader) 加载并转换 [CSON](https://github.com/bevry/cson#what-is-cson) 文件

### 语法转换

- [`babel-loader`](https://webpack.docschina.org/loaders/babel-loader) 使用 [Babel](https://babeljs.io/) 加载 ES2015+ 代码并将其转换为 ES5
- [`buble-loader`](https://github.com/sairion/buble-loader) 使用 [Bublé](https://buble.surge.sh/guide/) 加载 ES2015+ 代码并将其转换为 ES5
- [`traceur-loader`](https://github.com/jupl/traceur-loader) 使用 [Traceur](https://github.com/google/traceur-compiler#readme) 加载 ES2015+ 代码并将其转换为 ES5
- [`ts-loader`](https://github.com/TypeStrong/ts-loader) 像加载 JavaScript 一样加载 [TypeScript](https://www.typescriptlang.org/) 2.0+
- [`coffee-loader`](https://webpack.docschina.org/loaders/coffee-loader) 像加载 JavaScript 一样加载 [CoffeeScript](http://coffeescript.org/)
- [`fengari-loader`](https://github.com/fengari-lua/fengari-loader/) 使用 [fengari](https://fengari.io/) 加载 Lua 代码
- [`elm-webpack-loader`](https://github.com/elm-community/elm-webpack-loader) 像加载 JavaScript 一样加载 [Elm](https://elm-lang.org/)

### 模板

- [`html-loader`](https://webpack.docschina.org/loaders/html-loader) 将 HTML 导出为字符串，需要传入静态资源的引用路径
- [`pug-loader`](https://github.com/pugjs/pug-loader) 加载 Pug 和 Jade 模板并返回一个函数
- [`markdown-loader`](https://github.com/peerigon/markdown-loader) 将 Markdown 编译为 HTML
- [`react-markdown-loader`](https://github.com/javiercf/react-markdown-loader) 使用 markdown-parse 解析器将 Markdown 编译为 React 组件
- [`posthtml-loader`](https://github.com/posthtml/posthtml-loader) 使用 [PostHTML](https://github.com/posthtml/posthtml) 加载并转换 HTML 文件
- [`handlebars-loader`](https://github.com/pcardune/handlebars-loader) 将 Handlebars 文件编译为 HTML
- [`markup-inline-loader`](https://github.com/asnowwolf/markup-inline-loader) 将 SVG/MathML 文件内嵌到 HTML 中。在将图标字体或 CSS 动画应用于 SVG 时，此功能非常实用。
- [`twig-loader`](https://github.com/zimmo-be/twig-loader) 编译 Twig 模板并返回一个函数
- [`remark-loader`](https://github.com/webpack-contrib/remark-loader) 通过 `remark` 加载 markdown，且支持解析内容中的图片

### 样式

- [`style-loader`](https://webpack.docschina.org/loaders/style-loader) 将模块导出的内容作为样式并添加到 DOM 中
- [`css-loader`](https://webpack.docschina.org/loaders/css-loader) 加载 CSS 文件并解析 import 的 CSS 文件，最终返回 CSS 代码
- [`less-loader`](https://webpack.docschina.org/loaders/less-loader) 加载并编译 LESS 文件
- [`sass-loader`](https://webpack.docschina.org/loaders/sass-loader) 加载并编译 SASS/SCSS 文件
- [`postcss-loader`](https://webpack.docschina.org/loaders/postcss-loader) 使用 [PostCSS](http://postcss.org/) 加载并转换 CSS/SSS 文件
- [`stylus-loader`](https://webpack.docschina.org/loaders/stylus-loader/) 加载并编译 Stylus 文件

### 框架

- [`vue-loader`](https://github.com/vuejs/vue-loader) 加载并编译 [Vue 组件](https://vuejs.org/v2/guide/components.html)
- [`angular2-template-loader`](https://github.com/TheLarkInn/angular2-template-loader) 加载并编译 [Angular](https://angular.io/) 组件

### Awesome

有关更多第三方 loader，请参阅 [awesome-webpack](https://webpack.docschina.org/awesome-webpack/#loaders) 中的列表。



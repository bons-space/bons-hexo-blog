---
title: Webpack面试题
date: 2021-9-3 17:54:58
cover: https://img.showydream.com/img/m2XSRQ-2019-07-04-01.png
description: Webpack Q&A笔记，持续更新
keywords: Webpack，面试题，面经
categories: 
  - tech
---

[[toc]]

## 对Webpack的理解，解决了什么问题？

`Webpack`是一个用于现代`JavaScript`应用程序的静态模块打包工具。

### 静态模块

这里的静态模块指的是开发阶段，可以被`webpack`直接引用的资源（可以直接被获取打包进`bundle.js`的资源）

当`webpack`处理应用程序时，它会在内部构建一个依赖图，此依赖图对应映射到项目所需的每个模块（不再局限于`js`文件），并生成一个或多个`bundle`

<img src="https://img.showydream.com/img/1CmJJS-image-20210827122030611.png" alt="image-20210827122030611" style="zoom:50%;" />

### webpack的能力和解决的问题

- 编译代码能力

  在开发过程中，我们经常会使用一些高级的特性来加快我们的开发效率或者安全性，比如通过`ES6`+`TypeScript`开发脚本逻辑，通过`sass`、`less`等方式来编写`css`样式代码。`webpack`可以通过编译的形式，把不同特性的代码编译成浏览器能识别的`ES5`语法或者`css`文件，解决浏览器兼容问题

  压缩代码，优化网站性能

- 模块整合能力

  提高性能，可维护性，解决浏览器频繁请求文件的问题

- 万物皆可模块化能力

  项目维护性增强，支持不同种类的前端模块类型，统一的模块化方案，所有资源文件的加载可以通过代码控制

## webpack的构建流程

### 运行流程

<img src="https://user-images.githubusercontent.com/26785201/89747816-fe344280-daf2-11ea-820a-6a1a99e34f14.png" alt="img"/>

`webpack`的运行过程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 首先会从配置文件和`Shell`语句中读取与合并参数，并初始化需要使用的插件和配置插件等执行环境所需要的参数。
2. 初始化完成后会调用`Complier`的`run`来真正启动`webpack`编译构建过程，`webpack`的构建流程包括`compile`、`make`、`build`、`seal`、`emit`阶段，执行完这些阶段就完成了构建过程。



### 初始化

#### entry-options 启动

从配置文件和 `Shell` 语句中读取与合并参数，得出最终的参数。

#### run 实例化

`compiler`：用上一步得到的参数初始化 `Compiler` 对象，加载所有配置的插件，执行对象的 `run` 方法开始执行编译

### 编译构建

#### entry 确定入口

根据配置中的 `entry` 找出所有的入口文件

#### make 编译模块

从入口文件出发，调用所有配置的 `Loader` 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理

#### build module 完成模块编译

经过上面一步使用 `Loader` 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系

#### seal 输出资源

根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 `Chunk`，再把每个 `Chunk` 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会

#### emit 输出完成

在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统

<img src="https://img.showydream.com/img/XRGqwK-image-20210903115330607.png" alt="image-20210903115330607" style="zoom:50%;" />


## webpack.config.js 配置Demo

```javascript
var path = require('path');
var node_modules = path.resolve(__dirname, 'node_modules');
var pathToReact = path.resolve(node_modules, 'react/dist/react.min.js');

module.exports = {
  // 入口文件，是模块构建的起点，同时每一个入口文件对应最后生成的一个 chunk。
  entry: {
    bundle: [
      'webpack/hot/dev-server',
      'webpack-dev-server/client?http://localhost:8080',
      path.resolve(__dirname, 'app/app.js')
    ]
  },
  // 文件路径指向(可加快打包过程)。
  resolve: {
    alias: {
      'react': pathToReact
    }
  },
  // 生成文件，是模块构建的终点，包括输出文件与输出路径。
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: '[name].js'
  },
  // 这里配置了处理各模块的 loader ，包括 css 预处理 loader ，es6 编译 loader，图片处理 loader。
  module: {
    loaders: [
      {
        test: /\.js$/,
        loader: 'babel',
        query: {
          presets: ['es2015', 'react']
        }
      }
    ],
    noParse: [pathToReact]
  },
  // webpack 各插件对象，在 webpack 的事件流中执行对应的方法。
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ]
};
```

除此之外再大致介绍下webpack的一些核心概念：

- ##### loader

  能转换各类资源，并处理成对应模块的加载器。

  loader间可以串行使用。

- ##### chunk

  code splitting后的产物，也就是按需加载的分块，装载了不同的module。

对于 module 和 chunk 的关系可以参照 webpack 官方的这张图：

![img](https://img.showydream.com/img/VXlztt-TB1B0DXNXXXXXXdXFXXXXXXXXXX-368-522.jpg)

- ##### plugin

  webpack插件的实体，这里以`UglifyJsPlugin`为例

  ```javascript
  function UglifyJsPlugin(options) {
    this.options = options;
  }
  
  module.exports = UglifyJsPlugin;
  
  UglifyJsPlugin.prototype.apply = function(compiler) {
    compiler.plugin("compilation", function(compilation) {
      compilation.plugin("build-module", function(module) {
      });
      compilation.plugin("optimize-chunk-assets", function(chunks, callback) {
        // Uglify 逻辑
      });
      compilation.plugin("normal-module-loader", function(context) {
      });
    });
  };
  ```

  在webpack中你经常可以看到`compilation.plugin('xxx',callback)`，你可以把它当做一个事件的绑定，这些事件在打包时由webpack来触发。

# webpack中的Loader总结

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

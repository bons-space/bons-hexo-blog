---
title: Webpack简介
date: 2021-9-3 17:54:58
updated: 2021-11-5 14:00:00
cover: https://img.showydream.com/img/m2XSRQ-2019-07-04-01.png
description: Webpack Q&A笔记，持续更新
keywords: Webpack，面试题，面经
tags: Webpack
categories: 
  - Webpack
---



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


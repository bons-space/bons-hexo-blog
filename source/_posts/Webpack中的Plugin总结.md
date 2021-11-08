---
title: Webpack中的Plugin总结
date: 2021-11-4 17:54:58
updated: 2021-11-5 14:00:00
cover: https://img.showydream.com/img/m2XSRQ-2019-07-04-01.png
description: Webpack Q&A笔记，持续更新
keywords: Webpack，面试题，面经
tags: Webpack
categories: 
  - Webpack
---

### Plugin介绍

顾名思义，plugin是插件的意思，它是一种遵循Webpack应用程序接口规范编写的程序，在webpack规定的系统下运行。plugin赋予其各种灵活的功能，例如打包优化、资源管理、环境变量注入等，它们会在webpack不同阶段（钩子函数、生命周期）中运行，贯穿了webpack的整个编译周期。

<img src="https://img.showydream.com/img/v34XjV-9a04ec40-a7c2-11eb-ab90-d9ae814b240d-20211105151809349.png" alt="img" style="zoom:50%;" />

插件目的在于解决 loader无法实现的**其他事**。Webpack 提供很多开箱即用的插件

### 用法

由于**插件**可以携带参数/选项，你必须在 webpack 配置中，向 `plugins` 属性传入一个 `new` 实例。

取决于你的 webpack 用法，对应有多种使用插件的方式。

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 访问内置的插件
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader',
      },
    ],
  },
  plugins: [
    new webpack.ProgressPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' }),
  ],
};
```

### 常用插件

| **Name**                                                     | **Description**                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`BabelMinifyWebpackPlugin`](https://v4.webpack.docschina.org/plugins/babel-minify-webpack-plugin) | 使用 [babel-minify](https://github.com/babel/minify)进行压缩 |
| [`BannerPlugin`](https://v4.webpack.docschina.org/plugins/banner-plugin) | 在每个生成的 chunk 顶部添加 banner                           |
| [`CommonsChunkPlugin`](https://v4.webpack.docschina.org/plugins/commons-chunk-plugin) | 提取 chunks 之间共享的通用模块                               |
| [`CompressionWebpackPlugin`](https://v4.webpack.docschina.org/plugins/compression-webpack-plugin) | 预先准备的资源压缩版本，使用 Content-Encoding 提供访问服务   |
| [`ContextReplacementPlugin`](https://v4.webpack.docschina.org/plugins/context-replacement-plugin) | 重写 `require` 表达式的推断上下文                            |
| [`CopyWebpackPlugin`](https://v4.webpack.docschina.org/plugins/copy-webpack-plugin) | 将单个文件或整个目录复制到构建目录                           |
| [`DefinePlugin`](https://v4.webpack.docschina.org/plugins/define-plugin) | 允许在编译时(compile time)配置的全局常量                     |
| [`DllPlugin`](https://v4.webpack.docschina.org/plugins/dll-plugin) | 为了极大减少构建时间，进行分离打包                           |
| [`EnvironmentPlugin`](https://v4.webpack.docschina.org/plugins/environment-plugin) | [`DefinePlugin`](https://v4.webpack.docschina.org/plugins/define-plugin) 中 `process.env` 键的简写方式。 |
| [`ExtractTextWebpackPlugin`](https://v4.webpack.docschina.org/plugins/extract-text-webpack-plugin) | 从 bundle 中提取文本（CSS）到单独的文件                      |
| [`HotModuleReplacementPlugin`](https://v4.webpack.docschina.org/plugins/hot-module-replacement-plugin) | 启用模块热替换(Enable Hot Module Replacement - HMR)          |
| [`HtmlWebpackPlugin`](https://v4.webpack.docschina.org/plugins/html-webpack-plugin) | 简单创建 HTML 文件，用于服务器访问                           |
| [`I18nWebpackPlugin`](https://v4.webpack.docschina.org/plugins/i18n-webpack-plugin) | 为 bundle 增加国际化支持                                     |
| [`IgnorePlugin`](https://v4.webpack.docschina.org/plugins/ignore-plugin) | 从 bundle 中排除某些模块                                     |
| [`LimitChunkCountPlugin`](https://v4.webpack.docschina.org/plugins/limit-chunk-count-plugin) | 设置 chunk 的最小/最大限制，以微调和控制 chunk               |
| [`LoaderOptionsPlugin`](https://v4.webpack.docschina.org/plugins/loader-options-plugin) | 用于从 webpack 1 迁移到 webpack 2                            |
| [`MinChunkSizePlugin`](https://v4.webpack.docschina.org/plugins/min-chunk-size-plugin) | 确保 chunk 大小超过指定限制                                  |
| [`MiniCssExtractPlugin`](https://v4.webpack.docschina.org/plugins/mini-css-extract-plugin) | 为每个引入 CSS 的 JS 文件创建一个 CSS 文件                   |
| [`NoEmitOnErrorsPlugin`](https://v4.webpack.docschina.org/configuration/optimization/#optimization-noemitonerrors) | 在输出阶段时，遇到编译错误跳过                               |
| [`NormalModuleReplacementPlugin`](https://v4.webpack.docschina.org/plugins/normal-module-replacement-plugin) | 替换与正则表达式匹配的资源                                   |
| [`NpmInstallWebpackPlugin`](https://v4.webpack.docschina.org/plugins/npm-install-webpack-plugin) | 在开发环境下自动安装缺少的依赖                               |
| [`ProgressPlugin`](https://v4.webpack.docschina.org/plugins/progress-plugin) | 报告编译进度                                                 |
| [`ProvidePlugin`](https://v4.webpack.docschina.org/plugins/provide-plugin) | 不必通过 import/require 使用模块                             |
| [`SourceMapDevToolPlugin`](https://v4.webpack.docschina.org/plugins/source-map-dev-tool-plugin) | 对 source map 进行更细粒度的控制                             |
| [`EvalSourceMapDevToolPlugin`](https://v4.webpack.docschina.org/plugins/eval-source-map-dev-tool-plugin) | 对 eval source map 进行更细粒度的控制                        |
| [`UglifyjsWebpackPlugin`](https://v4.webpack.docschina.org/plugins/uglifyjs-webpack-plugin) | 可以控制项目中 UglifyJS 的版本                               |
| [`TerserPlugin`](https://v4.webpack.docschina.org/plugins/terser-webpack-plugin) | 允许控制项目中 Terser 的版本                                 |
| [`ZopfliWebpackPlugin`](https://v4.webpack.docschina.org/plugins/zopfli-webpack-plugin) | 通过 node-zopfli 将资源预先压缩的版本                        |



### HtmlWebpackPlugin

在打包结束后，⾃动生成⼀个 `html` ⽂文件，并把打包生成的`js` 模块引⼊到该 `html` 中

```bash
npm install --save-dev html-webpack-plugin
```

```javascript
// webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
 ...
  plugins: [
     new HtmlWebpackPlugin({
       title: "My App",
       filename: "app.html",
       template: "./src/html/index.html"
     }) 
  ]
};
```

```html
<!--./src/html/index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title><%=htmlWebpackPlugin.options.title%></title>
</head>
<body>
    <h1>html-webpack-plugin</h1>
</body>
</html>
```

在 `html` 模板中，可以通过 `<%=htmlWebpackPlugin.options.XXX%>` 的方式获取配置的值

更多的配置可以自寻查找

### clean-webpack-plugin

删除（清理）构建目录

```bash
npm install --save-dev clean-webpack-plugin
```

```javascript
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
module.exports = {
 ...
  plugins: [
    ...,
    new CleanWebpackPlugin(),
    ...
  ]
}
```

### mini-css-extract-plugin

提取 `CSS` 到一个单独的文件中

```bash
npm install --save-dev mini-css-extract-plugin
```

```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
 ...,
  module: {
   rules: [
    {
     test: /\.s[ac]ss$/,
     use: [
      {
       loader: MiniCssExtractPlugin.loader
     },
          'css-loader',
          'sass-loader'
        ]
   }
   ]
 },
  plugins: [
    ...,
    new MiniCssExtractPlugin({
     filename: '[name].css'
    }),
    ...
  ]
}
```

### DefinePlugin

允许在编译时创建配置的全局对象，是一个`webpack`内置的插件，不需要安装

```javascript
const { DefinePlugun } = require('webpack')

module.exports = {
 ...
    plugins:[
        new DefinePlugin({
            BASE_URL:'"./"'
        })
    ]
}
```

这时候编译`template`模块的时候，就能通过下述形式获取全局对象

```html
<link rel="icon" href="<%= BASE_URL%>favicon.ico>"
```

### copy-webpack-plugin

复制文件或目录到执行区域，如`vue`的打包过程中，如果我们将一些文件放到`public`的目录下，那么这个目录会被复制到`dist`文件夹中

```text
npm install copy-webpack-plugin -D
```

1

```javascript
new CopyWebpackPlugin({
    parrerns:[
        {
            from:"public",
            globOptions:{
                ignore:[
                    '**/index.html'
                ]
            }
        }
    ]
})
```

复制的规则在`patterns`属性中设置：

- from：设置从哪一个源中开始复制
- to：复制到的位置，可以省略，会默认复制到打包的目录下
- globOptions：设置一些额外的选项，其中可以编写需要忽略的文件

# Weback使用小技巧



#### 批量导入文件：require.context

通过执行require.context函数获取一个特定的上下文,主要用来实现自动化导入模块,在前端工程中,如果遇到从一个文件夹引入很多模块的情况,可以使用这个api,它会遍历文件夹中的指定文件,然后自动导入,使得不需要每次显式的调用import导入模块

##### 使用语法

Webpack 会在构建中解析代码中的 `require.context()` 。

```javascript
require.context(
  directory,
  (useSubdirectories = true),
  (regExp = /^\.\/.*$/),
  (mode = 'sync')
);
```

示例：

```javascript
require.context('./test', false, /\.test\.js$/);
//（创建出）一个 context，其中文件来自 test 目录，request 以 `.test.js` 结尾。
require.context('../', true, /\.stories\.js$/);
// （创建出）一个 context，其中所有文件都来自父文件夹及其所有子级文件夹，request 以 `.stories.js` 结尾。
```

**require.context() 返回的结果是一个函数，该函数包含3个属性**

- resolve{Function} : 是一个函数，返回已分析请求的模块id
- keys : 返回上下文模块可以处理的所有可能请求的数组的函数
- id : 是上下文模块的模块id。这可能对module.hot.acce有用



使用示例：

```javascript
// vue store中 批量拿到module文件夹中的state文件
const modulesFiles = require.context('./modules', true, /\.js$/)
const modules = modulesFiles.keys().reduce((modules, modulePath) => {
  const moduleName = modulePath.replace(/^\.\/(.*)\.\w+$/, '$1')
  const value = modulesFiles(modulePath)
  modules[moduleName] = value.default
  return modules
}, {})
```


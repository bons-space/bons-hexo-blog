---
title: Weback使用小技巧
type: tags
cover: 'https://img.showydream.com/img/m2XSRQ-2019-07-04-01.png'
description: Webpack Q&A笔记，持续更新
keywords: Webpack，技巧，面经
tags: Webpack
categories:
  - Webpack
abbrlink: f39d325f
date: 2021-11-08 12:54:58
updated: 2021-11-08 17:00:00
---

### 批量导入文件：require.context

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


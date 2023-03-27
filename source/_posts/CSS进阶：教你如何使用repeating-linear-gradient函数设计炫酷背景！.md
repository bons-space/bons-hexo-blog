---
title: CSS进阶：教你如何使用repeating-linear-gradient函数设计炫酷背景！
cover: 'https://img.showydream.com/img/yLnAqE-jackson-sophat-_t-l5FFH8VA-unsplash.jpg'
tags: CSS
categories:
  - CSS
abbrlink: 8172d94b
date: 2023-03-22 14:39:21
---

当我们需要为网页元素设置背景时，通常可以使用一些渐变函数来实现各种效果。其中，`repeating-linear-gradient` 函数是一个非常实用的函数，它可以帮助我们快速创建一个重复的线性渐变背景。

### 什么是 `repeating-linear-gradient` 函数？

`repeating-linear-gradient` 是CSS中的一个渐变函数，它允许我们创建一个线性渐变，并将这个渐变重复应用在元素的背景上，从而形成一个连续的图案。该函数的语法如下：

```css
background: repeating-linear-gradient(direction, color-stop1, color-stop2, ...);
```

其中，`direction` 表示渐变的方向，可以使用关键词（如 `to right` 表示从左到右）或角度值（如 `45deg` 表示从左上角到右下角）来指定。`color-stop` 表示颜色和位置信息，可以使用颜色值或者 `transparent` 等特殊值，以及位置值（如 `50%` 表示在渐变中间位置）。

### 例子1：横条纹背景

下面的代码将创建一个从左到右的线性渐变，渐变颜色为红色到蓝色，然后将这个渐变重复应用在元素的背景上，形成一个横条纹的图案：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b458e0301af4b3aa913603a7717fe39~tplv-k3u1fbpfcp-watermark.image?)

```css
width: 400px;
height: 100px;
background: repeating-linear-gradient(to right, red, blue 50%);
```

在这个例子中，`to right` 表示渐变的方向是从左到右，`red` 和 `blue` 分别表示起点和终点的颜色，`50%` 表示在中间位置时颜色为蓝色。

### 例子2：斜条纹背景

下面的代码将创建一个从左上到右下的线性渐变，渐变颜色为红色到蓝色，然后将这个渐变重复应用在元素的背景上，形成一个斜条纹的图案：


![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f41d8fb993e8425389c850849075c32c~tplv-k3u1fbpfcp-watermark.image?)

```css
width: 400px;
height: 100px;
background: repeating-linear-gradient(45deg, red, blue 50%);
```

在这个例子中，`45deg` 表示渐变的方向是从左上角到右下角，其他参数与例子1相同。

### 例子3：方格背景

下面的代码将创建一个从左上到右下的线性渐变，渐变颜色为白色到灰色，然后将这个渐变重复应用在元素的背景上，并且通过 `background-size` 属性来控制渐变的大小和重复次数，从而形成一个方格背景：


![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/488ab7a7a2a94881a15f19e2b67dea01~tplv-k3u1fbpfcp-watermark.image?)

```css
width: 200px;
height: 200px;
background: repeating-linear-gradient(45deg, white, white 25%, grey 25%, grey 50%);
background-size: 20px 20px;
```

在这个例子中，`25%`表示在渐变的1/4处时为白色，渐变的1/2处时为灰色，其他参数与例子2相同。`background-size` 属性中的 `20px 20px` 表示将背景大小设置为 20 像素 × 20 像素，即每个方格的大小为 20 像素 × 20 像素。

### 总结

`repeating-linear-gradient` 函数是一个非常实用的 CSS 渐变函数，它可以帮助我们快速创建各种连续的线性渐变图案，比如条纹、格子等等。掌握这个函数的基本用法可以让我们更好地掌控网页的视觉效果，提升用户体验。
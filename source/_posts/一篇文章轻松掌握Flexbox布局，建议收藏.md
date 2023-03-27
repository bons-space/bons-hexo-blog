---
title: 一篇文章轻松掌握Flexbox布局，建议收藏
cover: 'https://img.showydream.com/img/yLnAqE-jackson-sophat-_t-l5FFH8VA-unsplash.jpg'
tags: CSS
categories:
  - CSS
abbrlink: 4c68c92e
date: 2023-03-24 14:42:46
---

Flexbox 是 CSS 中一种强大的布局模式，它使得可以轻松地进行网页设计，并创建响应式页面布局。Flexbox 布局提供了一种灵活的方式，可以使 HTML 元素在容器中进行自适应排列。在这篇文章中，我们将探讨 Flexbox 和一些与其相关的属性。

# 什么是 Flexbox？
Flexbox 是 CSS 中的一种布局模式，用于实现容器内项目的弹性盒模型。它使开发人员可以轻松地定义一个容器，并将元素放入其中，自动进行对齐和布局。通过使用 Flexbox，我们可以实现各种复杂的布局，而不需要使用传统的 float 或 position 属性。

Flexbox 布局中的元素被分为两类：容器（flex container）和项目（flex item）。容器是包含项目的元素，而项目是容器中放置的元素。在 Flexbox 布局中，容器定义了一个 "弹性容器"，其子元素是 "弹性项目"。弹性容器可以在任何方向上调整其子元素的大小和位置。

# Flexbox 父元素上设置的相关属性
1. display: flex; （设置容器为 Flexbox 布局）

将容器设置为 Flexbox 布局，使得容器内的子元素可以使用 Flexbox 布局的相关属性进行排列和对齐。

2. flex-direction: row | row-reverse | column | column-reverse; （设置主轴方向）

- row：水平方向，起点在左端。
- row-reverse：水平方向，起点在右端。
- column：垂直方向，起点在上沿。
- column-reverse：垂直方向，起点在下沿。

3. justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly; （设置主轴上的对齐方式）

- flex-start：主轴起点对齐。
- flex-end：主轴终点对齐。
- center：主轴居中对齐。
- space-between：平均分布在主轴上，首尾子元素贴近容器两侧。
- space-around：平均分布在主轴上，子元素之间的间距相等，首尾子元素与容器边缘的距离是子元素之间距离的一半。
- space-evenly：平均分布在主轴上，子元素之间的间距相等，首尾子元素与容器边缘的距离相等。

4. align-items: flex-start | flex-end | center | baseline | stretch; （设置交叉轴上的对齐方式）

- flex-start：交叉轴起点对齐。
- flex-end：交叉轴终点对齐。
- center：交叉轴居中对齐。
- baseline：基线对齐。
- stretch：子元素在交叉轴上被拉伸至容器的高度（默认值）。

5. flex-wrap: nowrap | wrap | wrap-reverse; （设置子元素是否换行）

- nowrap：默认值，不换行。
- wrap：子元素在一行排不下时换行。
- wrap-reverse：子元素在一行排不下时换行，且换行后的排列顺序反转。

6. align-content: flex-start | flex-end | center | space-between | space-around | stretch; （设置多行子元素在交叉轴上的对齐方式）

- flex-start：多行子元素的交叉轴起点对齐。
- flex-end：多行子元素的交叉轴终点对齐。
- center：多行子元素的交叉轴中心对齐。
- space-between：多行子元素在交叉轴上平均分布，首尾子元素贴近容器两侧。
- space-around：多行子元素在交叉轴上平均分布，子元素之间的间距相等，首尾子元素与容器边缘的距离是子元素之间距离的一半。
- stretch：多行子元素在交叉轴上被拉伸至容器的高度。
- flex-flow: `<flex-direction>` || `<flex-wrap>`; （flex-direction 和 flex-wrap 的缩写）

用于同时设置 flex-direction 和 flex-wrap 的属性值，中间用两个竖线分隔。

7. gap: `<length>`; （设置子元素之间的间距）

8. justify-items: flex-start | flex-end | center | baseline | stretch; （设置所有子元素在主轴上的对齐方式）

## Flexbox 子元素上设置的相关属性

1. flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]

- flex-grow: 定义在分配多余空间时，子元素的放大比例，默认为0，不放大。
- flex-shrink: 定义在空间不足时，子元素的缩小比例，默认为1，可缩小。
- flex-basis: 定义在没有设置宽度或高度时，子元素在主轴上的基准值。

  以上三个值的缩写可以直接写成一个 flex 值，例如：flex: 1; 等价于 flex: 1 1 0%;

2. order: `<integer>`; （设置子元素的排列顺序，默认为0，表示按照文档流的顺序排列）

3. align-self: auto | flex-start | flex-end | center | baseline | stretch; （设置单个子元素在交叉轴上的对齐方式）

# Flexbox 的例子

下面是一些使用 Flexbox 布局的例子：

## 居中对齐


![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a58d9c47970c409ab0e6b080165fb4f2~tplv-k3u1fbpfcp-watermark.image?)

```html
<div class="container">
  <div class="item">Item 1</div>
  <div class="item">Item 2</div>
  <div class="item">Item 3</div>
</div>
```
```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}

.item {
  width: 100px;
  height: 100px;
  background-color: red;
  margin: 10px;
}
```
在上面的例子中，我们将容器设置为 flex 布局，并使用 justify-content: center 和 align-items: center 居中对齐项目。

## 自适应列数


![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6fa3b5faf347445280b68c077c07a472~tplv-k3u1fbpfcp-watermark.image?)

```html
<div class="container">
  <div class="item">Item 1</div>
  <div class="item">Item 2</div>
  <div class="item">Item 3</div>
  <div class="item">Item 4</div>
  <div class="item">Item 5</div>
</div>
```
```css
.container {
  display: flex;
  flex-wrap: wrap;
}

.item {
  flex: 1 0 25%;
  height: 100px;
  background-color: red;
  margin: 10px;
}
```
在上面的例子中，我们将容器设置为 flex 布局，并使用 flex-wrap: wrap 换行。然后，我们使用 flex: 1 0 25% 将项目的弹性因子设置为 1，基础大小设置为 25%，这样就可以自动适应不同的列数（这里第一行展示3个子元素是因为设置了margin）。

## 自适应高度

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4fd21ee62b7b4a5fb7d986bfc25992b6~tplv-k3u1fbpfcp-watermark.image?)

```html
    <div class="container">
      <div class="item">Item 1</div>
      <div class="item">Item 2</div>
      <div class="item">Item 3</div>
    </div>
```
```css
.container {
  display: flex;
  align-items: stretch;
  height: 300px;
}
.item {
  flex: 1;
  background-color: red;
  margin: 10px;
}
```
在上面的例子中，我们将容器设置为 flex 布局，并使用 align-items: stretch 将项目的高度拉伸以适应容器的高度。这样，无论容器的高度如何，项目的高度都会自适应。


# Flexbox vs Grid
除了 Flexbox，还有另一个强大的 CSS 布局模型，即 Grid。虽然两者都可以创建复杂的布局，但它们有不同的用途和优势。

1. Flexbox 适用于一维布局，例如在水平或垂直方向上对项目进行排序和对齐。它特别适合于创建导航菜单、列表、排版和响应式设计等布局。
Grid 适用于二维布局，即在行和列上对项目进行排序和对齐。它特别适合于创建网格布局、复杂的表格和多列布局。
以下是 Flexbox 和 Grid 的一些比较：

2. Flexbox 可以使用 flex-direction 属性来控制项目的排序方向，而 Grid 可以使用 grid-template-columns 和 grid-template-rows 属性来定义行和列。

3. Flexbox 可以使用 justify-content 和 align-items 属性来对齐项目，而 Grid 可以使用 justify-items 和 align-items 属性来对齐单个项目。

4. Flexbox 的主轴和交叉轴是动态的，而 Grid 的行和列是静态的。这意味着在 Flexbox 中，项目的大小可以根据内容自动调整，而在 Grid 中，行和列的大小是固定的。

虽然两者之间存在一些差异，但它们都是非常强大和有用的布局工具，可以大大简化我们的 CSS 布局代码。

# 最后

Flexbox 是一种强大的布局模式，对响应式布局也比较友好，具体使用的时候还要根据业务场景灵活掌握。

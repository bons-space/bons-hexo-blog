---
title: 前端BFC
cover: >-
  https://img.showydream.com/img/iZX9MU-picography-food-platters-beach-restaurant-small-768x512.jpg
description: >-
  BFC（Block formatting context）直译为“块级格式化上下文”。它是一个独立的渲染区域，只有Block-level
  box参与，它规定了内部的Block-level box如何布局，并且与这个区域外部毫不相干。
keywords: BFC
tags: 面经
categories:
  - 前端
abbrlink: 104be820
date: 2021-04-02 12:00:00
---



我们在页面布局的时候，经常会出现以下情况：

- 这个元素高度怎么没了？
- 这两栏布局怎么没法自适应？
- 这两个元素的间距怎么有点奇怪的样子？
- 。。。

归根到底是元素之间的互相影响，导致了预期之外的情况的发生。这里就涉及到了BFC的概念

## BFC的概念

 BFC（Block formatting context）直译为“块级格式化上下文”。它是一个独立的渲染区域，只有Block-level box参与，它规定了内部的Block-level box如何布局，并且与这个区域外部毫不相干。

- BFC是一个独立的布局环境，可以理解为一个容器，在这个容器中按照一定的规则进行物品摆放，并且不会影响其他环境中的物品。
- 如果一个元素符合出发BFC的条件，则BFC中的元素布局不受外部影响。
- 如果浮动元素会创建BFC，则浮动元素内部的子元素都受到该浮动元素的影响，所以浮动元素之间是互不影响的。

## BFC的特性

1. BFC是一个独立的容器，容器内子元素不会影响容器外的元素，反之亦是如此。
2. 盒子从顶端开始垂直的一个一个的排列，盒子之间垂直的间距是由margin决定的。
3. 在同一个BFC中，两个相邻的块级盒子的垂直外边距会发生重叠。
4. BFC区域不会和float box发生重叠。
5. BFC能够识别并包含浮动元素，当计算其区域的高度时，浮动元素也可以参与计算了。
6. 计算BFC的高度时，浮动元素也参与计算。

## 触发BFC

只要元素满足以下任意一种条件就可以触发BFC特性：

- body根元素
- 浮动元素：float除none以外的值
- 绝对定位元素：position（absolute、fixed）
- display为inline-block、table-cells、flex、inline-flex、table-caption
- overflow除了visible以外的值（hidden、auto、scroll）

## BFC的应用

1. #### 防止margin重叠（塌陷）

   **相邻**的两个盒子（可能是兄弟关系也可能是祖先关系）的垂直边距相遇时， 它们将形成一个外边距。这个外边距的高度等于两个发生折叠的外边距的高度中的**较大者**。

   **外边距折叠的条件是 margin 必须相邻!**

   ```html
   <style type="text/css">
       .box{
           color: brown;
           background: rgb(226, 159, 108);
           width: 100px;
           height: 100px;
           text-align: center;
           line-height: 100px;
           margin: 100px;
       }
   </style>
   
   <body>
       <div class="box">上面</div>
       <div class="box">下面</div>
   </body>
   ```

   <img src="https://img.showydream.com/img/eeQvIh-image-20210406144515012.png" alt="image-20210406144515012" style="zoom:50%;" />

   两个box之间的距离为100px，发生了margin重叠（塌陷），解决这个问题，只需要把其中一个box变为BFC元素即可

   ```html
   <style type="text/css">
       .box {
           color: brown;
           background: rgb(226, 159, 108);
           width: 100px;
           height: 100px;
           text-align: center;
           line-height: 100px;
           margin: 100px;
       }
   </style>
   
   <body>
       <div class="box">上面</div>
       <div style="overflow: hidden;">
           <div class="box">下面</div>
       </div>
   </body>
   ```

   <img src="https://img.showydream.com/img/sABvEs-image-20210406145329212.png" alt="image-20210406145329212" style="zoom:50%;" />

   

2. #### 清除内部浮动

   - 浮动元素会脱离文档流(绝对定位元素也会脱离文档流)，导致无法计算准确的高度，这种问题称为**高度塌陷**。
   - 解决高度塌陷问题的前提是能够识别并包含浮动元素，也就是**清除浮动**。

   ```html
   <style type="text/css">
       .parent {
           width: 300px;
           border: 2px solid rgb(120, 201, 177);
       }
       .box {
           color: brown;
           background: rgb(226, 159, 108);
           width: 100px;
           height: 100px;
           text-align: center;
           line-height: 100px;
           float: left;
       }
   </style>
   <body>
       <div class="parent">
           <div class="box">上面</div>
           <div class="box">下面</div>
       </div>
   </body>
   ```

   <img src="https://img.showydream.com/img/UkhPhY-image-20210406150234363.png" alt="image-20210406150234363" style="zoom:50%;" />

   如上左图所示，容器（parent）没有高度或者 height = auto ,并且其子元素（box）是浮动元素，所以该容器的高度是不会被撑开的，即高度塌陷。

   解决方法：**在容器（container）中创建 BFC。**

   ```html
   
   ```
<style type="text/css">
       .parent {
           width: 300px;
           border: 2px solid rgb(120, 201, 177);
           overflow: hidden;
       }
       .box {
           color: brown;
           background: rgb(226, 159, 108);
           width: 100px;
           height: 100px;
           text-align: center;
           line-height: 100px;
           float: left;
       }
   </style>

   <body>
       <div class="parent">
           <div class="box">上面</div>
           <div class="box">下面</div>
       </div>
   </body>
   ```
   
   <img src="https://img.showydream.com/img/dKS7cW-image-20210406150535638.png" alt="image-20210406150535638" style="zoom:50%;" />

3. #### 自适应多栏布局

   ```html
   <style type="text/css">
       body{
           width: 300px;
       }
       .aside {
           width: 100px;
           height: 150px;
           float: left;
           background: rgb(47, 187, 145);
       }
       .main {
           height: 200px;
           background: rgb(171, 75, 156);
       }
   </style>
   <body>
       <div class="aside">aside</div>
       <div class="main">main</div>
   </body>
   ```

   <img src="https://img.showydream.com/img/7OF8JS-image-20210406151256736.png" alt="image-20210406151256736" style="zoom:50%;"/>

   这时候main元素有部分被浮动元素所覆盖，(但是文本信息不会被浮动元素所覆盖) 如果想避免元素被覆盖，可以通过触发`main`生成`BFC`，以此适应两栏布局

   ```html
   <style type="text/css">
       body{
           width: 300px;
       }
       .aside {
           width: 100px;
           height: 150px;
           float: left;
           background: rgb(47, 187, 145);
       }
       .main {
           height: 200px;
           overflow: hidden;
           background: rgb(171, 75, 156);
       }
   </style>
   
   <body>
       <div class="aside">aside</div>
       <div class="main">main</div>
   </body>
   ```

<img src="https://img.showydream.com/img/z1IwLr-image-20210406151547943.png" alt="image-20210406151547943" style="zoom:50%;" />



## 参考链接

https://juejin.cn/post/6844903780253696013#heading-5

https://zhuanlan.zhihu.com/p/25321647

https://segmentfault.com/a/1190000013647777

https://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html

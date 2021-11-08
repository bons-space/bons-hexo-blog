---
title: CSS画图大法
date: 2021-04-18 09:38:42
cover: https://img.showydream.com/img/RTpMv6-11c39f7be6854b6b92b0180ec7524baa~tplv-k3u1fbpfcp-watermark.image
description: CSS画几何图形
tags: 面经
keywords: CSS画几何图形，CSS画三角形
categories: 
  - CSS
---


### 用css画一个三角形

思路：把div的宽和高都设为0，然后设置四个border的颜色。

```html
<style type="text/css">
    .triangle {
        width: 0;
        height: 0;
        border-top: 50px solid blue;
        border-right: 50px solid red;
        border-bottom: 50px solid green;
        border-left: 50px solid yellow;
    }
</style>
<body style="padding: 40px;">
    <div class="triangle"></div>
</body>
```

<img src="https://img.showydream.com/img/5DpIXX-image-20210415154000125.png" alt="image-20210415154000125" style="zoom:50%;" />

然后用css的transparent属性把其他三个三角的颜色变透明，这样我们就得到一个三角形了。

```html
<style type="text/css">
    .triangle {
        width: 0;
        height: 0;
        border: 50px solid transparent;
        border-bottom: 50px solid green;
    }
</style>

<body style="padding: 40px;">
    <div class="triangle"></div>
</body>
```

<img src="https://img.showydream.com/img/ebrrvl-image-20210415154229146.png" alt="image-20210415154229146" style="zoom:50%;" />

如果下面的bottom宽一点，三角形就会变尖

```html
<style type="text/css">
    .triangle {
        width: 0;
        height: 0;
        border: 50px solid transparent;
        border-bottom: 90px solid green;
    }
</style>
<body style="padding: 40px;">
    <div class="triangle"></div>
</body>
```

<img src="https://img.showydream.com/img/fBo0xT-image-20210415154422217.png" alt="image-20210415154422217" style="zoom:50%;" />

如果想要一个空心的三角形，可以在后面加一个伪类：

```html
<style type="text/css">
    .triangle {
        width: 0;
        height: 0;
        border: 50px solid transparent;
        border-bottom: 50px solid green;
        position: relative;
    }
    .triangle:after{
        content: '';
        border: 40px solid transparent;
        border-bottom: 40px solid #fff;
        position: absolute;
        right: -40px;
        top: -33px;
    }
</style>
<body style="padding: 40px;">
    <div class="triangle"></div>
</body>
```

<img src="https://img.showydream.com/img/eNfspe-image-20210415155254357.png" alt="image-20210415155254357" style="zoom:50%;" />

画完三角形，有点意犹未尽，那么问题来了，如何用css画其他的几何图形？

### 用css画一个平行四边形

画平行四边形的思路跟画三角形有点不一样，这时候我们用到了**CSS**的**transform**的**skew**方法。[在这里](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/skew())看MDN的文档

```html
<style type="text/css">
    .parallelogram {
        width: 150px;
        height: 100px;
        transform: skew(25deg);
        background: green;
    }
</style>
<body style="padding: 40px;">
    <div class="parallelogram"></div>
</body>
```

<img src="https://img.showydream.com/img/ZVGOFO-image-20210415162035076.png" alt="image-20210415162035076" style="zoom:50%;" />

### 用CSS画一个梯形

画梯形的思路跟画三角形一样，区别是给div加一点宽度：

```html
<style type="text/css">
  .trapezoid{
        width: 100px;
        height: 0;
        border: 50px solid transparent;
        border-bottom: 50px solid green;
        position: relative;
    }
</style>
<body style="padding: 40px;">
    <div class="trapezoid"></div>
</body>
```

<img src="https://img.showydream.com/img/Aw1RR6-image-20210415162554039.png" alt="image-20210415162554039" style="zoom:50%;" />

### 用CSS画一个六角形

画六角形的思路还是用三角形的方式，在伪类里画一个倒三角形，然后用绝对定位把伪类放到合适的位置。

点歌：六等星の夜

```html
<style type="text/css">
  .star{
        width: 0;
        height: 0;
        border: 60px solid transparent;
        border-bottom: 100px solid green;
        position: relative;
    }
    .star:after {
        content: '';
    		border: 60px solid transparent;
    		border-top: 100px solid green;
    		position: absolute;
    		right: -60px;
    		top: 33px;
    }
</style>
<body style="padding: 40px;">
    <div class="star"></div>
</body>
```

<img src="https://img.showydream.com/img/yYRJoJ-image-20210415163151870.png" alt="image-20210415163151870" style="zoom:50%;" />

### 用CSS画一个五角星

画五角星的思路和画六角形一样，区别是一个伪类不够用了，这回before也用上，还有就是需要用到transform的rotate()函数。[MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate())

```html
<style type="text/css">
  .star-five {
      position: relative;
      width: 0;
      height: 0;
      border-right: 100px solid transparent;
      border-bottom: 70px solid green;
      border-left: 100px solid transparent;
      transform: rotate(35deg);
    }
    .star-five:before {
      border-bottom: 80px solid green;
      border-left: 30px solid transparent;
      border-right: 30px solid transparent;
      position: absolute;
      height: 0;
      width: 0;
      top: -45px;
      left: -65px;
      content: '';
      transform: rotate(-35deg);
    }
    .star-five:after {
      position: absolute;
      display: block;
      color: green;
      top: 3px;
      left: -105px;
      width: 0;
      height: 0;
      border-right: 100px solid transparent;
      border-bottom: 70px solid green;
      border-left: 100px solid transparent;
      transform: rotate(-70deg);
      content: '';
    }
</style>
<body style="padding: 80px;">
    <div class="star-five"></div>
</body>
```

<img src="https://img.showydream.com/img/73GW5b-image-20210415164731705.png" alt="image-20210415164731705" style="zoom:50%;" />

### 用CSS画一个扇形

思路如上，话不多说，直接上代码：

```html
<style type="text/css">
  .sector {
        width: 0px;
        height: 0px;
        border-right: 60px solid transparent;
        border-top: 60px solid green;
        border-left: 60px solid green;
        border-bottom: 60px solid green;
        border-top-left-radius: 60px;
        border-top-right-radius: 60px;
        border-bottom-left-radius: 60px;
        border-bottom-right-radius: 60px;
    }
</style>
<body style="padding: 80px;">
    <div class="sector"></div>
</body>
```



几何图形的练习到此结束，我们发现带角的这种几何图形都可以用border去画出来，主要是思路灵活一点，脑洞大一点就可以了。下面我们玩儿点花的

### 用CSS画一个月亮

这回用到了新思路：`box-shadow`

```html
<style type="text/css">
   .moon {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      box-shadow: 15px 15px 0 0 green;
    }
</style>
<body style="padding: 80px;">
    <div class="moon"></div>
</body>
```

<img src="https://img.showydream.com/img/LPTTTQ-image-20210415171918291.png" alt="image-20210415171918291" style="zoom:50%;" />

### 用css画一个小心心

画小心心的思路与上面画几何图形类似，区别是这回不用border了，先画一个头部是圆形，底部是方形的半颗心，并将它旋转45度

```html
<style type="text/css">
		.heart {
      position: relative;
    }
    .heart:before {
      position: absolute;
      content: "";
      left: 50px;
      top: 0;
      width: 50px;
      height: 80px;
      background: red;
      border-radius: 50px 50px 0 0;
      transform: rotate(-45deg);
      transform-origin: 0 100%;
    }
</style>
<body style="padding: 80px;">
    <div class="heart"></div>
</body>
```

<img src="https://img.showydream.com/img/5V2OzZ-image-20210415170946156.png" alt="image-20210415170946156" style="zoom:50%;" />

然后在画一个同样的图形，反方向旋转45度。我们的小心心就画好啦。

```html
<style type="text/css">
  	.heart {
      position: relative;
    }
    .heart:before, .heart:after{
      position: absolute;
      content: "";
      left: 50px;
      top: 0;
      width: 50px;
      height: 80px;
      background: red;
      border-radius: 50px 50px 0 0;
      transform: rotate(-45deg);
      transform-origin: 0 100%;
    }
    .heart:after {
      left: 0;
      transform: rotate(45deg);
      transform-origin: 100% 100%;
    }
</style>
<body style="padding: 80px;">
    <div class="heart"></div>
</body>
```

<img src="https://img.showydream.com/img/yBp2AP-image-20210415171126723.png" alt="image-20210415171126723" style="zoom:50%;" />

### 用CSS画一个阴阳鱼

这个思路就比较好玩儿了，还是用到了伪类叠加的方式。首先画一个半边黑半边白的圆形。

```html
<style type="text/css">
  .yin-yang {
      width: 96px;
      box-sizing: content-box;
      height: 48px;
      background: #fff;
      border-color: #333;
      border-style: solid;
      border-width: 1px 1px 50px 1px;
      border-radius: 100%;
      position: relative;
    }
</style>
<body style="padding: 80px;">
    <div class="yin-yang"></div>
</body>
```

<img src="https://img.showydream.com/img/tNuqC0-image-20210415174858818.png" alt="image-20210415174858818" style="zoom:50%;" />

然后画一个黑色的鱼头，白色眼睛。

```
<style type="text/css">
  .yin-yang {
      width: 96px;
      box-sizing: content-box;
      height: 48px;
      background: #fff;
      border-color: #333;
      border-style: solid;
      border-width: 1px 1px 50px 1px;
      border-radius: 100%;
      position: relative;
    }
    .yin-yang:before {
      content: "";
      position: absolute;
      top: 50%;
      left: 0;
      background: #fff;
      border: 18px solid #333;
      border-radius: 100%;
      width: 12px;
      height: 12px;
      box-sizing: content-box;
    }
</style>
<body style="padding: 80px;">
    <div class="yin-yang"></div>
</body>
```

<img src="https://img.showydream.com/img/R1KR1q-image-20210415175018831.png" alt="image-20210415175018831" style="zoom:50%;" />

最后画一个白色鱼头，黑色眼睛：

```
<style type="text/css">
  .yin-yang {
      width: 96px;
      box-sizing: content-box;
      height: 48px;
      background: #fff;
      border-color: #333;
      border-style: solid;
      border-width: 1px 1px 50px 1px;
      border-radius: 100%;
      position: relative;
    }
    .yin-yang:before {
      content: "";
      position: absolute;
      top: 50%;
      left: 0;
      background: #fff;
      border: 18px solid #333;
      border-radius: 100%;
      width: 12px;
      height: 12px;
      box-sizing: content-box;
    }
    .yin-yang:after {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      background: #333;
      border: 18px solid #fff;
      border-radius: 100%;
      width: 12px;
      height: 12px;
      box-sizing: content-box;
    }
</style>
<body style="padding: 80px;">
    <div class="yin-yang"></div>
</body>
```

<img src="https://img.showydream.com/img/jFgYyG-image-20210415175131227.png" alt="image-20210415175131227" style="zoom:50%;" />

### 最后

祝大家看的开心，喜欢帮忙点个关注呀

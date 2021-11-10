---
title: CSS增加用户体验的特效
cover: 'https://img.showydream.com/img/2kx9Pj-mo-nyqVAJLweq8-unsplash.jpg'
description: CSS增加用户体验的特效
keywords: 半透明边框，
tags: CSS
categories:
  - CSS
abbrlink: '54282336'
date: 2021-11-09 15:24:47
---



总结一下比较好玩儿的特效，用的时候直接来CV。长期更新

# 边框与背景

## 半透明边框

> 背景知识 [background-clip](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-clip)

默认情况下，背景的颜色会延伸至边框下层，这意味着我们设置的透明边框效果会被覆盖掉，在CSS3中，我们可以通过设置`background-clip: padding-box`来改变背景的默认行为，打到我们想要的效果

```html
<style>
    main {
        width: 100%;
        height: 100vh;
        background: #4b7b7c;
        padding-top: 100px;
    }
    div {
        padding: 12px;
        margin: 20px auto;
        width: 200px;
        background: #c7b723;
        border: 10px solid hsla(0, 0%, 100%, .5);
    }
    .default {
        margin-top: 20px;
        background-clip: padding-box;
    }
</style>
<body>
    <main>
        <div>默认背景</div>
        <div class="default">透明边框背景</div>
    </main>
</body>
```

效果如下：

<img src="https://img.showydream.com/img/3r1LeU-image-20211109154443635.png" alt="image-20211109154443635" style="zoom:50%;" />

## 多重边框

> 背景知识：[box-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow), [outline](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline),[outline-offset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline-offset)

`box-shadow`相信很多人都已经用透了，可用来给元素添加各种阴影效果，反过来，也只有我们需要实现阴影时才会想起它，其实，`box-shadow`还接受第四个参数作为阴影扩张半径，当我们只设置扩张半径时，零偏移，零模糊，产生的效果其实相当于一条实线“**边框**”。

`box-shadow`只能模拟实线边框效果，某些情况下，我们可能需要生成虚线的边框效果，我们可以通过类似于`border`的描边`outline`和对应的描边偏移`outline-offset`来实现。

```html
<style>
    main {
        width: 100%;
        padding: 0 10vh;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
    }
    div:nth-of-type(1) {
        width: 60px;
        height: 60px;
        border-radius: 50%;
        background: #c0faf5;
        margin: 105px 29px;
        box-shadow: 0 0 0 10px #ace0db, 0 0 0 20px #94c0bc,
            0 0 0 30px #83aaa7, 0 0 0 40px #6b928f,
            0 0 0 50px #598884, 0 0 0 60px #3b7973,
            0 0 0 70px #2b746d, 0 0 0 80px #1a5f59;
    }
    div:nth-of-type(2) {
        width: 200px;
        height: 120px;
        background: #a3d6d2;
        border: 5px solid #4b7b7c;
        margin-left: 200px;
        outline: #4b7b7c dashed 1px;
        outline-offset: -10px;
        margin-bottom: 20px;
    }
</style>
<body>
    <main>
        <div></div>
        <div></div>
    </main>
</body>
```

<img src="https://img.showydream.com/img/mDsSqb-image-20211109162826882.png" alt="image-20211109162826882" style="zoom:50%;" />

## 动图条纹进度条

> 背景知识[repeating-linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/gradient/repeating-linear-gradient())、[animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)

```html
<style>
    main {
        width: 800px;
        padding: 80px 0px;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
    }
    .progress-outer {
        width: 60%;
        height: 12px;
        border-radius: 8px;
        overflow: hidden;
        position: relative;
    }
    .progress-enter {
        height: inherit;
        background: #d6ecea;
    }
    .progress-bg {
        width: 60%;
        height: inherit;
        border-radius: 6px;
        background: repeating-linear-gradient(-45deg, #ace0db 25%, #1a5f59 0, #ace0db 50%,
                #1a5f59 0, #ace0db 75%, #1a5f59 0);
        background-size: 16px 16px;
        animation: progressAnimation 20s linear infinite;
    }
    @keyframes progressAnimation {
        to {
            background-position: 200% 0;
        }
    }
</style>
<body>
    <main>
        <div class="progress-outer">
            <!--更好的扩展-->
            <div class="progress-enter">
                <div class="progress-bg"></div>
            </div>
            <!-- <span>60%</span> -->
        </div>
    </main>
</body>
```

效果是动态的，这里懒得截动图了

<img src="https://img.showydream.com/img/IzgZ9f-image-20211109173350055.png" alt="image-20211109173350055" style="zoom:50%;" />


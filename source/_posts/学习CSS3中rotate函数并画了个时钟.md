---
title: 学习CSS3中rotate函数并画了个时钟
cover: >-
  https://img.showydream.com/img/uSzhc3-white-wall-clock-on-blue-background-picture-id1289661784.jpeg
keywords: 'Vue3,时钟,钟表,CSS'
tags: CSS技巧
categories:
  - CSS
abbrlink: f7d249b0
date: 2021-11-11 10:54:58
description:
---

这天逛博客看到好多大佬的网站上都有时钟贼拉有格调，就想自己也整一个，想到作为一个前端开发人员，体现前端开发技术的时刻到了<(￣︶￣)>，用CSS画一个！先给大家看效果：

<img src="https://img.showydream.com/img/49h5zN-2zxdi-sirwe.gif" alt="img" style="zoom: 67%;" />

# 前置知识

下面来总结一下用到的知识点：

## [rotateZ()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateZ())函数

`rotateZ()`这个CSS 函数定义了将元素在Z轴（就是数学坐标系X轴，Y轴，Z轴里的Z轴）上旋转而不使其变形的方法。 其运动的程度由指定的角度来定义；如果是正的，则为顺时针旋转，如果是负的，则是逆时针旋转。

## [rotate()](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate())函数

<img src="https://developer.mozilla.org/@api/deki/files/5976/=transform-functions-rotate_19.5.png" alt="img" style="zoom:;" />

The `rotate()` CSS 函数 定义一个旋转属性，将元素在不变形的情况下旋转到不动点周围(如 [`transform-origin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin) 属性所指定) 。 移动量由指定角度定义;如果为正值，则运动将为顺时针，如果为负值，则为逆时针 。 180°的旋转称为点反射 (*point reflection*)。

## [`transform-origin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)属性

**`transform-origin`** CSS属性让你更改一个元素变形的原点。

# 时钟实现

1. 画出表的圆心

   这里我用背景垂直水平居中+`before`伪类直接在背景上画出来的

2. 画出时针、分针、秒针，并给它们添加定时任务，使其转起来

   这里就用到了`rotateZ()`函数，核心代码如下

   ```html
   <template>
     <div class="clock">
       <!--时针-->
       <div class="hour">
         <div ref="hr" class="hr"></div>
       </div>
       <!--分针-->
       <div class="min">
         <div ref="mn" class="mn"></div>
       </div>
       <!--秒针-->
       <div class="sec">
         <div ref="sc" class="sc"></div>
       </div>
     </div>
   </template>
   <script lang="ts">
     import { ref, onMounted, onUnmounted } from 'vue'
     export default {
       setup() {
         const hr = ref<HTMLElement | null>(null)
         const mn = ref<HTMLElement | null>(null)
         const sc = ref<HTMLElement | null>(null)
         const timeChange = () => {
           let day = new Date()
           let hh = day.getHours() * 30 // 一小时转30度
           let mm = day.getMinutes() * 6 // 一个刻度
           let ss = day.getSeconds() * 6 // 一个刻度
           ;(hr.value as HTMLElement).style.transform = `rotateZ(${hh + mm / 12}deg)`
           ;(mn.value as HTMLElement).style.transform = `rotateZ(${mm}deg)`
           ;(sc.value as HTMLElement).style.transform = `rotateZ(${ss}deg)`
         }
         let timer: any = null
         onMounted(() => {
           timeChange()
           timer = setInterval(timeChange, 1000)
         })
         onUnmounted(() => {
           timer && window.clearInterval(timer)
         })
   
         return {
           hr,
           mn,
           sc,
         }
       }
     }
   </script>
   
   <style scoped lang="scss">
     .clock {
       width: 300px;
       height: 300px;
       border-radius: 50%;
       display: flex;
       justify-content: center;
       align-items: center;
       position: relative;
   
       &:before {
         content: '';
         width: 15px;
         height: 15px;
         border-radius: 50%;
         position: absolute;
         z-index: 10;
       }
   
       .hour,
       .min,
       .sec {
         position: absolute;
       }
   
       .hour,
       .hr {
         width: 160px;
         height: 160px;
       }
   
       .min,
       .mn {
         width: 200px;
         height: 200px;
       }
   
       .sec,
       .sc {
         width: 230px;
         height: 230px;
       }
   
       .hr,
       .mn,
       .sc {
         display: flex;
         justify-content: center;
         border-radius: 50%;
       }
   
       .hr::before {
         content: '';
         width: 8px;
         height: 80px;
         //background-color: #b03232;
         border-radius: 6px;
       }
   
       .mn::before {
         content: '';
         width: 4px;
         height: 95px;
         //background-color: #fff;
         border-radius: 6px;
       }
   
       .sc::before {
         content: '';
         width: 2px;
         height: 150px;
         //background-color: #fff;
         border-radius: 4px;
       }
     }
   
   </style>
   
   ```

   实际的旋转中，是一个整个元素在旋转，而不是一个指针

   <img src="https://img.showydream.com/img/TAvHmO-image-20211111115250680.png" alt="image-20211111115250680" style="zoom:50%;" />

3. 画出表盘背景

   这里是画时钟最复杂的地方，如果只用`rotate()`和`transform-origin`属性，我们会发现表盘刻度变成了这样

   <img src="https://img.showydream.com/img/IgBA2w-image-20211111115602912.png" alt="image-20211111115602912" style="zoom:50%;" />

   这是为啥呢？原因很简单，因为在遍历过程中，每个刻度都有自己的高度，随着高度的累计，每个刻度的旋转的原点不一样了，简单来说就是圆心跟着动了。那么解决方案也就来了，只需要用`position: absolute`让它们的圆心一样就好了。
   
   
   
# 全部代码

```html
<template>
  <div class="light">
    <div class="clock">
      <ul class="scaleBg">
        <li v-for="i in 60" :key="i" class="scale" :style="scale(i)"></li>
        <li v-for="i in 12" :key="i" class="scaleNum" :style="scaleNum(i)">
          <span :style="fScaleNum(i)">{{ i }}</span>
        </li>
      </ul>
      <div class="hour">
        <div ref="hr" class="hr"></div>
      </div>
      <div class="min">
        <div ref="mn" class="mn"></div>
      </div>
      <div class="sec">
        <div ref="sc" class="sc"></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
  import { ref, onMounted, onUnmounted } from 'vue'
  export default {
    setup() {
      const hr = ref<HTMLElement | null>(null)
      const mn = ref<HTMLElement | null>(null)
      const sc = ref<HTMLElement | null>(null)

      const timeChange = () => {
        let day = new Date()
        let hh = day.getHours() * 30 // 一小时转30度
        let mm = day.getMinutes() * 6 // 一个刻度
        let ss = day.getSeconds() * 6 // 一个刻度
        ;(hr.value as HTMLElement).style.transform = `rotateZ(${hh + mm / 12}deg)`
        ;(mn.value as HTMLElement).style.transform = `rotateZ(${mm}deg)`
        ;(sc.value as HTMLElement).style.transform = `rotateZ(${ss}deg)`
      }

      let timer: any = null

      onMounted(() => {
        timeChange()
        timer = setInterval(timeChange, 1000)
      })
      onUnmounted(() => {
        timer && window.clearInterval(timer)
      })

      const scale = (i: number) => {
        if (i === 0 || i % 5 === 0) {
          return {
            transform: `rotate(${i * 6}deg)`,
            height: '16px'
          }
        } else {
          return {
            transform: `rotate(${i * 6}deg)`
          }
        }
      }
      const scaleNum = (i: number) => {
        if (i === 12) {
          return {
            left: '146px',
            transform: `rotate(${i * 30}deg)`
          }
        } else {
          return {
            transform: `rotate(${i * 30}deg)`
          }
        }
      }

      const fScaleNum = (i: number) => {
        // 把表盘刻度的字摆正
        return {
          display: 'block',
          transform: `rotate(${-i * 30}deg)`
        }
      }

      return {
        hr,
        mn,
        sc,
        scale,
        scaleNum,
        fScaleNum
      }
    }
  }
</script>

<style scoped lang="scss">
  .clock {
    width: 300px;
    height: 300px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    //box-shadow: 0 -15px 15px rgba(255, 255, 255, 0.05), inset 0 -15px 15px rgba(255, 255, 255, 0.05),
    //  0 15px 15px rgba(255, 255, 255, 0.05), inset 0 15px 15px rgba(255, 255, 255, 0.05);

    &:before {
      content: '';
      width: 15px;
      height: 15px;
      border-radius: 50%;
      position: absolute;
      //background: #fff;
      z-index: 10;
    }

    .hour,
    .min,
    .sec {
      position: absolute;
    }

    .hour,
    .hr {
      width: 160px;
      height: 160px;
    }

    .min,
    .mn {
      width: 200px;
      height: 200px;
    }

    .sec,
    .sc {
      width: 230px;
      height: 230px;
    }

    .hr,
    .mn,
    .sc {
      display: flex;
      justify-content: center;
      border-radius: 50%;
    }

    .hr::before {
      content: '';
      width: 8px;
      height: 80px;
      //background-color: #b03232;
      border-radius: 6px;
    }

    .mn::before {
      content: '';
      width: 4px;
      height: 95px;
      //background-color: #fff;
      border-radius: 6px;
    }

    .sc::before {
      content: '';
      width: 2px;
      height: 150px;
      //background-color: #fff;
      border-radius: 4px;
    }
  }

  .scaleBg {
    position: relative;
    width: 100%;
    height: 100%;

    .scale {
      width: 2px;
      height: 6px;
      background: #fff;
      position: absolute;
      left: 150px;
      top: 0;
      transform-origin: center 150px;
    }
    .scaleNum {
      position: absolute;
      display: flex;
      justify-content: center;
      left: 148px;
      top: 30px;
      font-size: 12px;
      transform-origin: center 120px;
    }
  }

  .light {
    .clock {
      box-shadow: 0 -15px 15px rgba(255, 255, 255, 0.05),
        inset 0 -15px 15px rgba(255, 255, 255, 0.05), 0 15px 15px rgba(255, 255, 255, 0.05),
        inset 0 15px 15px rgba(255, 255, 255, 0.05);

      &:before {
        background: #181818;
      }
      .hr::before {
        background-color: #b03232;
      }
      .mn::before {
        background-color: #525252;
      }
      .sc::before {
        background-color: #383737;
      }
      .scale {
        background: #181818;
      }
    }
  }

  .dark {
    .clock {
      box-shadow: 0 -15px 15px rgba(255, 255, 255, 0.05),
        inset 0 -15px 15px rgba(255, 255, 255, 0.05), 0 15px 15px rgba(255, 255, 255, 0.05),
        inset 0 15px 15px rgba(255, 255, 255, 0.05);

      &:before {
        background: #fff;
      }
      .hr::before {
        background-color: #b03232;
      }
      .mn::before {
        background-color: #fff;
      }
      .sc::before {
        background-color: #fff;
      }
      .scale {
        background: #fff;
      }
    }
  }
</style>
```

# 总结

技术要灵活运用呀，CSS真强呀！！

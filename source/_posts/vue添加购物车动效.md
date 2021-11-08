---
title: vue添加购物车动效
date: 2021-4-13 17:54:58
cover: https://img.showydream.com/img/NSxdnY-post-image2.jpg
description: 用vue实现添加购物车特效。
keywords: vue实现添加购物车特效
tags: Vue技巧
categories: 
  - Vue
---


今天搞了个好玩儿的功能，添加到购物车特效，话不多说，先上效果

<img src="https://img.showydream.com/img/pmcULm-0bwnr-2y73w.gif" alt="0bwnr-2y73w" style="zoom:50%;" />

实现这个效果有几个要点如下：

- 购物车car在一个固定的位置，我是用`position:fixed`把购物车固定在右边
- 小球动态的轨迹需要用到贝塞尔曲线，[在这个网站上可以调试](https://cubic-bezier.com/#.4,-0.89,.55,.54)，注意贝塞尔曲线是小球运动的快慢，不是轨迹（看了半天文章才看明白）。
- 小球的位置跟购物车的位置一样，当抛的时候要算出添加购物车的点到购物车的transform横坐标位移和纵坐标位移，然后把小球拉到当前位置，用transition动画加贝塞尔曲线展示。

核心代码如下

```html

<!--小球-->
<div>
   <transition
     name="drop"
     @before-enter="beforeDrop"
     @enter="dropping"
     @after-enter="afterDrop"
   >
     <div
       v-show="ball.show"
       class="ball"
     >
       <div class="inner inner-hook" />
     </div>
   </transition>
</div>

<!--购物车的位置-->
<div class="rangeIcon">
    {{ count }}    
 </div>

<script>
 export default {
   data(){
     return{
       ball: {
        show: false,
        el: null 
       },
       count: 0
     }
   },
   methods:{
     drop (el) { // 抛物
      if (!this.ball.show) {
        this.ball.show = true
        this.ball.el = el
      }
    },
    beforeDrop (el) { /* 购物车小球动画实现 */
      const ball = this.ball
      if (ball.show) {
        const rect = ball.el.getBoundingClientRect() // 元素相对于视口的位置
        const x = -(window.innerWidth - rect.left - 10) // 获取x
        const y = -(window.innerHeight - rect.top - 430) // 获取y
        el.style.display = ''
        el.style.webkitTransform = 'translateY(' + y + 'px)' // translateY
        el.style.transform = 'translateY(' + y + 'px)'
        const inner = el.getElementsByClassName('inner-hook')[0]
        inner.style.webkitTransform = 'translateX(' + x + 'px)'
        inner.style.transform = 'translateX(' + x + 'px)'
      }
    },
    dropping (el, done) { /* 样式重置 */
      const rf = el.offsetHeight
      el.style.webkitTransform = 'translate3d(0,0,0)'
      el.style.transform = 'translate3d(0,0,0)'
      const inner = el.getElementsByClassName('inner-hook')[0]
      inner.style.webkitTransform = 'translate3d(0,0,0)'
      inner.style.transform = 'translate3d(0,0,0)'
      el.addEventListener('transitionend', done)
    },
    afterDrop (el) { /* 初始化小球 */
      this.ball.show = false
      el.style.display = 'none'
    },
   }
 }
</script>  

<style scoped lang="scss">
  .ball {
  position: fixed;
  right: 10px;
  bottom: 430px;
  z-index: 200;
  transition: all 1s cubic-bezier(.4, -0.89, .55, .54);
  /*贝塞尔曲线*/
}

.inner {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #FE6E6B;
  transition: all 1s linear;
}
  
.rangeIcon {
  position: fixed;
  right: 10px;
  bottom: 430px;
  font-size: 30px;
  color: #DA3937;
  cursor: pointer;
  width: 40px;
  height: 40px;
  background: #EFEFEF;
  box-shadow: 0 3px 7px 0 rgba(107, 107, 107, 0.35);
  border-radius: 2px;
  text-align: center;
  padding-top: 3px;
}  
</style>
```

由于业务原因，我的添加按钮在购物车当前组件的子组件，和子子组件，所以搞了个[emitter](https://www.npmjs.com/package/emitt)去跟购物车组件通信。调用的时候子组件把`event`的`target`传到`drop`方法，小球就可以动起来啦。



参考：https://www.cnblogs.com/pengfei25/p/11736427.html


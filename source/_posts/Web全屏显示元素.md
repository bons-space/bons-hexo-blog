---
title: Web全屏显示元素
cover: 'https://img.showydream.com/img/5FrLAi-liisi-v-V5lTNwjnDGs-unsplash.jpg'
description: Web全屏显示元素 vue全屏
keywords: Web全屏显示元素，vue全屏
tags: Vue技巧
categories:
  - Vue
abbrlink: c0d0ca1a
date: 2021-12-03 15:02:10
---

```html
 <template>
  <main>
    <section class="left">left</section>
    <section ref="fullScreenEle" class="main-content">
      main-content
      <el-button @click="handleFullScreen">全屏切换</el-button>
    </section>
    <section class="right">right</section>
  </main>
</template>

<script lang="ts">
  import { ref } from 'vue'
  // 局部全屏
  export default {
    name: 'FullScreen',
    setup() {
      const isFull = ref<Boolean>(false)
      const fullScreenEle = ref<HTMLElement | null>(null)

      const handleFullScreen = () => {
        if (!document.fullscreenEnabled) {
          console.log('你的浏览器不支持全屏模式！')
          return
        }
        if (isFull.value) {
          if (document.exitFullscreen) {
            document.exitFullscreen()
          } else if (document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen()
          } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen()
          }
        } else {
          // 谷歌浏览器 firefox
          if (fullScreenEle.value && fullScreenEle.value.requestFullscreen) {
            fullScreenEle.value.requestFullscreen()
          }
          // safari浏览器
          else if (fullScreenEle.value && (fullScreenEle.value as HTMLElement).webkitRequestFullScreen) {
            fullScreenEle.value.webkitRequestFullScreen()
          }
          // 兼容浏览器 IE11
          else if (fullScreenEle.value && (fullScreenEle.value as HTMLElement).mozRequestFullscreen) {
            fullScreenEle.value.mozRequestFullscreen()
          }
        }
        isFull.value = !isFull.value
      }

      return {
        isFull,
        fullScreenEle,
        handleFullScreen
      }
    }
  }
</script>

<style scoped lang="scss">
  main {
    width: 100%;
    padding: 20px;
    display: flex;
    height: calc(100vh - 200px);

    .left,
    .right {
      width: 300px;
      background: #2b91af;
      color: #fff;
      font-size: 34px;
      height: 100%;
      text-align: center;
    }

    .main-content {
      flex: 1;
      height: 100%;
      background: #248459;
      color: #fff;
      font-size: 34px;
    }
  }
</style>
```



### 效果如下：

<img src="https://img.showydream.com/img/UEKUd9-iShot2021-12-03-15.09.23.gif" alt="iShot2021-12-03-15.09.23" style="zoom:67%;" />

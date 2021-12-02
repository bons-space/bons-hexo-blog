---
title: Vue跳转页面锚点
cover: 'https://img.showydream.com/img/47xgal-matt-wang-T7K0oKILrf0-unsplash.jpg'
description: Vue跳转页面锚点
keywords: Vue跳转页面锚点 scrollIntoView函数 Vue使用scrollIntoView
tags: Vue技巧
categories:
  - Vue
abbrlink: ba2243e3
date: 2021-11-30 18:58:57
---

## 正文

主要是用**scrollIntoView**函数，但是当页面有吸顶的header时定位到锚点的时候header会盖住一部分，scrollIntoView函数没办法直接解决这个问题，这里的破题思路是直接滚动一下页面的滚动条，让它往下滚动一点点，代码如下

```javascript
jumpAnchor (anchor) {
      if (this.$refs[anchor]) {
        this.$refs[anchor].scrollIntoView(true)
        this.$nextTick(() => {
          document.documentElement.scrollTop = -130 + this.$refs[anchor].offsetTop
        })
      }
 }
```

## 总结

解决这个问题的时候思路一度陷入用scrollIntoView函数解决问题的僵局，但是scrollIntoView函数根本就不支持解决这个问题。要做到善用工具，打开思路。

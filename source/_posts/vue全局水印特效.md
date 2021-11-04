---
title: vue全局水印特效
date: 2021-8-12 11:54:58
cover: https://img.showydream.com/img/NSxdnY-post-image2.jpg
description: vue全局水印特效
keywords: vue全局水印特效
categories: 
  - Vue
---

代码如下

```javascript
/**  水印添加方法  */

let setWatermark = (str1) => {
    let id = '532432.5435435324.543543543'

    if (document.getElementById(id) !== null) {
        document.body.removeChild(document.getElementById(id))
    }

    let can = document.createElement('canvas')
    // 设置canvas画布大小
    can.width = 300
    can.height = 200

    let cans = can.getContext('2d')
    cans.rotate(-20 * Math.PI / 180) // 水印旋转角度
    cans.font = '14px Vedana'
    cans.fillStyle = '#b6b3b3'
    cans.textAlign = 'center'
    cans.textBaseline = 'middle'
    cans.fillText(str1, can.width / 2, can.height) // 水印在画布的位置x，y轴

    let div = document.createElement('div')
    div.id = id
    div.style.pointerEvents = 'none'
    div.style.top = '54px'
    div.style.left = '0px'
    div.style.opacity = '0.15'
    div.style.position = 'fixed'
    div.style.zIndex = '100000'
    div.style.width = document.documentElement.clientWidth + 'px'
    div.style.height = document.documentElement.clientHeight  + 'px'
    div.style.background = 'url(' + can.toDataURL('image/png') + ') left top repeat'
    document.body.appendChild(div)
    return id
}

// 添加水印方法
export const setWaterMark = (str1) => {
    let id = setWatermark(str1)
    if (document.getElementById(id) === null) {
        id = setWatermark(str1)
    }
}

// 移除水印方法
export const removeWatermark = () => {
    let id = '532432.5435435324.543543543'
    if (document.getElementById(id) !== null) {
        document.body.removeChild(document.getElementById(id))
    }
}
```

用法

```javascript
  mounted () {
    setWaterMark('测试水印')
  },
  destroyed () {
    removeWatermark()
  }
```

效果

<img src="https://img.showydream.com/img/MJ68Vz-image-20210812112945707.png" alt="image-20210812112945707" style="zoom:50%;" />

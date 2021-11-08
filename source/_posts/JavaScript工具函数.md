---
title: JavaScript工具函数
date: 2021-5-16 00:00:00
cover: https://img.showydream.com/img/ZrtTjb-javascript.jpg
description: JavaScript工具函数，持续更新。
keywords: JavaScript, function, tools
tags: JavaScript工具函数
categories: 
 - Javascript
---


### 处理字符串中的html标签

```javascript
// 处理字符串中的html标签
export const escapeHtml = function (str) {
  let temp = document.createElement("div");
  (temp.textContent != null) ? (temp.textContent = str) : (temp.innerText = str)
  const output = temp.innerHTML
  temp = null
  // 用于v-html高亮过滤
  return output
    .replace(/@highlighted-field@/g, '<em>')
    .replace(/@\/highlighted-field@/g, '</em>') || ''
}
```

### 数字转换成单位数

```javascript
//  120000 => 10万 120001 => 10万+
export const transformNum = (value) => {
  if (isNaN(parseInt(value))) {
    return 0
  }
  const val = parseInt(value)
  if (val < 1000) {
    return val
  }
  let fr = 1000
  let num = 4
  let unit = '千'
  while (val / fr >= 1) {
    if (val / fr < 10) {
      break
    }
    fr *= 10
    num += 1
  }
  switch (num) {
    case 4:
      unit = '千'
      break
    case 5:
      unit = '万'
      break
    case 6:
      unit = '0万'
      break
    case 7:
      unit = '百万'
      break
    case 8:
      unit = '千万'
      break
    case 9:
      unit = '亿'
      break
    case 10:
      unit = '0亿'
      break
    case 11:
      unit = '百亿'
      break
    case 12:
      unit = '千亿'
      break
    default:
      break
  }
  return `${Math.floor(val / fr)}${unit}${val % fr > 0 ? '+' : ''}`
}
```

### 文件大小计算

```javascript
export const getFileSize = (value) => {
  if (isNaN(parseInt(value))) {
    return ''
  }
  const size = parseInt(value)

  const num = 1024.00 // byte

  if (size < num) { return size + "B" }
  if (size < Math.pow(num, 2)) { return (size / num).toFixed(2) + "K" } // kb
  if (size < Math.pow(num, 3)) { return (size / Math.pow(num, 2)).toFixed(2) + "M" } // M
  if (size < Math.pow(num, 4)) { return (size / Math.pow(num, 3)).toFixed(2) + "G" } // G
  return (size / Math.pow(num, 4)).toFixed(2) + "T" // T
}
```

### 获取字符数

```javascript
// 一个中文两个字符
export const getByteLen = (val) => {
  let len = 0
  for (let i = 0; i < val.length; i++) {
    const iLen = val.charCodeAt(i)
    if (iLen >= 0 && iLen <= 128) {
      len += 1
    } else {
      len += 2
    }
  }
  return len
}
```

### 图片url转base64

```javascript
export const getBase64Image = (url, cb) => {
  const image = new Image()
  image.src = url + "?v" + Math.random() // 处理缓存
  image.crossOrigin = '*' // 支持跨域
  image.onload = () => {
    const base64 = drawBase64Image(image)
    cb && cb(base64)
  }
}
const drawBase64Image = (img) => {
  const canvas = document.createElement('canvas')
  canvas.width = img.width
  canvas.height = img.height
  const ctx = canvas.getContext('2d')
  ctx.drawImage(img, 0, 0, img.width, img.height)
  return canvas.toDataURL('image/png')
}
```

### 复制文本

```javascript
export const msgCopy = (value) => {
  if (!value) {
    console.error('无复制内容')
    return
  }
  // 动态创建 textarea 标签
  const textarea = document.createElement('textarea')
  // 将该 textarea 设为 readonly 防止 iOS 下自动唤起键盘，同时将 textarea 移出可视区域
  textarea.readOnly = 'readonly'
  textarea.style.position = 'absolute'
  textarea.style.left = '-9999px'
  // 将要 copy 的值赋给 textarea 标签的 value 属性
  textarea.value = value
  // 将 textarea 插入到 body 中
  document.body.appendChild(textarea)
  // 选中值并复制
  textarea.select()
  // textarea.setSelectionRange(0, textarea.value.length);
  const result = document.execCommand('Copy')
  if (result) {
    Message.success('复制成功')
  }
  document.body.removeChild(textarea)
}
```

### 格式化时间

```JavaScript
export function formatTime (time) {
  const addZero = (m) => {
    return m < 10 ? '0' + m : m
  }
  if ((`${time}`).length === 10) {
    time = window.parseInt(time) * 1000
  } else {
    time = +time
  }
  const d = new Date(time)
  return (
    `${d.getFullYear()}-${addZero(d.getMonth() + 1)}-${addZero(d.getDate())} ${addZero(d.getHours())}:${addZero(d.getMinutes())}:${addZero(d.getSeconds())}`
  )
}

export function formatMinute (time) {
  const addZero = (m) => {
    return m < 10 ? '0' + m : m
  }
  if ((`${time}`).length === 10) {
    time = window.parseInt(time) * 1000
  } else {
    time = +time
  }
  const d = new Date(time)
  return (
    `${d.getFullYear()}.${addZero(d.getMonth() + 1)}.${addZero(d.getDate())} ${addZero(d.getHours())}:${addZero(d.getMinutes())}`
  )
}

export const formatDate = (time) => {
  const addZero = (m) => {
    return m < 10 ? '0' + m : m
  }
  if ((`${time}`).length === 10) {
    time = window.parseInt(time) * 1000
  } else {
    time = +time
  }
  const d = new Date(time)
  return (
    `${d.getFullYear()}.${addZero(d.getMonth() + 1)}.${addZero(d.getDate())}`
  )
}
```


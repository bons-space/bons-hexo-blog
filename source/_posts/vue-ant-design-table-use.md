---
title: Ant Design Vue中Table组件行内右键菜单实现方法
date: 2021-08-04 19:00:00
cover: https://img.showydream.com/img/nvpJTu-ant-design-vue.jpeg
description: Ant Design Vue中Table组件行内右键菜单实现方法
keywords: vue,Ant Design
categories: 
 - js
---

思路就是把右键菜单放到一个全局隐藏的地方，监听`<a-table>`的行的响应事件，动态控制右键菜单的位置和展示。

```vue
<template>
	<main>
  	<a-table 
      :columns="columns"
      :data-source="tableData"
      :custom-row="customRow"
    />
    <a-menu
      v-if="menuVisible"
      :style="menuStyle"
    >
      <a-menu-item>查看</a-menu-item>
      <a-menu-item>删除</a-menu-item>
    </a-menu>
  </main>
</template>

<script>
  export default {
    data(){
      return{
        columns:[],
        tableData:[]
      }
    },
    methods:{
      customRow(record, index) {
      	return {
        	on: {
          	contextmenu: e => {
            	e.preventDefault()
            	this.menuData = record
            	this.menuVisible = true
            	this.menuStyle.top = e.clientY + "px"
            	this.menuStyle.left = e.clientX + "px"
            	document.body.addEventListener("click", this.cancelClick)
          	}
        	}
      	}
    	},
    	cancelClick () {
      	this.menuVisible = false
      	document.body.removeEventListener("click", this.cancelClick)
    	}
    }
  }
</script>
```



效果如下

<img src="https://img.showydream.com/img/s5IzVh-image-20210804192925157.png" alt="image-20210804192925157" style="zoom:50%;" />


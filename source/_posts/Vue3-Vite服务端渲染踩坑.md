---
title: Vue3+Vite服务端渲染踩坑
cover: 'https://img.showydream.com/img/jaDOPF-photo-1638913972776-873fc7af3fdf.jpeg'
description: Vue3+Vite服务端渲染踩坑
keywords: Vue3+Vite服务端渲染踩坑
tags: 服务端渲染
categories:
  - Vue
abbrlink: 457c1806
date: 2021-12-13 19:46:18
---

### 找不到名称“require”。是否需要为节点安装类型定义? 请尝试使用 `npm i --save-dev @types/node`，然后将 “node” 添加到类型字段。ts(2591)

#### 解决方法

安装`@types/node`依赖包，并在`tsconfig.json`文件中**compilerOptions**的types配置中增加'node'

```json
"compilerOptions": {
     // ...
     "types": [
         "vite/client",
         "node"
     ],
 		 // ...
 }
```

### SyntaxError: Cannot use import statement outside a module

elementPlus自动引包导致的错误，手动引包解决

### 路由配置

```javascript
export function createRouter() {
  return _createRouter({
    history: import.meta.env.SSR ? createMemoryHistory() : createWebHistory(),
    routes
  })
}
```

一定要这么配置，不这么配置要不服务端渲染不出来，要不页面路由不变化，参数加不上去。

### vue3.0中监听路由router变化

vue3.0中的监听路由已经不能使用watch的方法，改进方式，使用 onBeforeRouteUpdate

```javascript
import { onBeforeRouteUpdate } from "vue-router";

onBeforeRouteUpdate(to => {
      console.log(to);
});
```

#### Vue3 获取this

> [getcurrentinstance()](https://v3.cn.vuejs.org/api/composition-api.html#getcurrentinstance)方法

```javascript
import { getCurrentInstance } from 'vue'
const { ctx } = getCurrentInstance() as any

onMounted(() => {
  nextTick(() => {
    if (ctx.$refs['line-0']) {
      firstLineHeight.value = ctx.$refs['line-0'][0].offsetHeight
    }
  })
})

```

#### Vue3` <script setup>` 如何调用子组件的方法和变量

使用 `<script setup>` 的组件是**默认关闭**的，也即通过模板 ref 或者 `$parent` 链获取到的组件的公开实例，不会暴露任何在 `<script setup>` 中声明的绑定。

为了在 `<script setup>` 组件中明确要暴露出去的属性，使用 `defineExpose` 编译器宏：

```html
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```

当父组件通过模板 ref 的方式获取到当前组件的实例，获取到的实例会像这样 `{ a: number, b: number }` (ref 会和在普通实例中一样被自动解包)


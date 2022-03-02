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

坑！！！`getCurrentInstance`代表全局上下文，ctx相当于Vue2的this,但是特别注意ctx代替this只适用于开发阶段，等你放到服务器上运行就会出错，上线的话需要用proxy替代ctx

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

### [vite] http proxy error: Error: socket hang up

错误输出：

<img src="https://img.showydream.com/img/i2pQp5-image-20211220150200618.png" alt="image-20211220150200618" style="zoom:50%;" />

这不是vite或者vue3的问题，请检查代码有没有死循环（我的就是死循环导致页面一直刷新=，=）



### ReferenceError: document is not defined

引的依赖包中默认引入document、window对象，会导致服务端渲染时找不到对象

不能够出现 `require/module` 等 `commonjs` 关键字，可使用 [createRequire](http://nodejs.cn/api/module.html#modulecreaterequirefilename) 方法自定义require

```javascript
import Module from 'module';
const require = Module.createRequire(import.meta.url);
```



解决方法：写一个client-only组件，我是抄的nuxtjs的，[源码链接](https://github.com/nuxt/framework/blob/4e9a27257bdfae65403ea73116d8c5508b642f44/packages/nuxt3/src/app/components/client-only.mjs)

```javascript
import { ref, onMounted, defineComponent, createElementBlock } from 'vue'

export default defineComponent({
  name: 'ClientOnly',
  // eslint-disable-next-line vue/require-prop-types
  props: ['fallback', 'placeholder', 'placeholderTag', 'fallbackTag'],
  setup (_, { slots }) {
    const mounted = ref(false)
    onMounted(() => { mounted.value = true })
    return (props) => {
      if (mounted.value) { return slots.default?.() }
      const slot = slots.fallback || slots.placeholder
      if (slot) { return slot() }
      const fallbackStr = props.fallback || props.placeholder || ''
      const fallbackTag = props.fallbackTag || props.placeholderTag || 'span'
      return createElementBlock(fallbackTag, null, fallbackStr)
    }
  }
})
```

### v-if尽量不要用！！！会导致服务端和客户端元素位置对不齐，客户端激活时出错。

### vite中elemtntPlus设置语言由于不能用commonjs语法，需要在项目中自己定义一份语言包，直接抄elementPlus中的即可

### 报错

<img src="https://img.showydream.com/img/v4iaJN-image-20220107152700608.png" alt="image-20220107152700608" style="zoom:50%;" />

解决：把配置文件的base改为'/'

<img src="https://img.showydream.com/img/mWkj61-image-20220107153836723.png" alt="image-20220107153836723" style="zoom:50%;" />

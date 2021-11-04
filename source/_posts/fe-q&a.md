---
title: 面试题
date: 2021-4-22 17:54:58
cover: https://img.showydream.com/img/iZX9MU-picography-food-platters-beach-restaurant-small-768x512.jpg
description: vueQ&A笔记，持续更新
keywords: Vue，面试题，面经
categories: 
  - tech
---

[[toc]]

## vue父子组件的执行顺序

当父子组件加载时，vue父子组件的生命周期的执行顺序为

**父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mounted**

**然后理解下这个顺序：**

1.当父组件执行完beforeMount挂载开始后，会依次执行子组件中的钩子，直到全部子组件mounted挂载到实例上，父组件才会进入mounted钩子

2.子级触发事件，会先触发父级beforeUpdate钩子，再去触发子级beforeUpdate钩子，下面又是先执行子级updated钩子，后执行父级updated钩子

总结：

父组件先于子组件created，而子组件先于父组件mounted

父子组件加载渲染过程：

父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

子组件更新：父beforeUpdate->子beforeUpdate->子updated->父updated

父组件更新过程：父beforeUpdate->父updated

销毁：父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

## Vue data为什么是函数而不是对象？

js的实例是通过构造函数来创建的，每个构造函数都可以new出很多实例，每个实例都会继承原型上的属性和方法。

**vue** 的每一个组件都是一个**vue**实例，**vue**的**data**数据是存在**vue**的原型上的属性，数据存在于内存中。**vue**为了保证数据之间的独立性，规定了**data**必须使用函数定义。如果**data**是一个对象的话，就会组件之间的相互影响，因为对象是对内存地址的引用，也就是说当一个组件的**data**值变化了，在其他组件的**data**值也会跟着变。造成组件之间的相互影响。

## ['1', '2', '3'].map(parseInt) what & why ?

第一反应是返回[1,2,3],但是真正的答案是`[1,NaN,NaN]`。问题在于parseInt接受两个参数，第一个是要被解析的值，第二个表示要解析的几进制，map的callback接受三个参数，第一个是当前正在处理的参数，第二个是参数的索引，第三个是map方法调用的数组。这时候会把map的前两个参数传到parseInt的接收的参数中，导致解析的结果为`[1,NaN,NaN]`

## Set、Map、WeakSet、和WeakMap的区别？

- #### Set

  对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

- #### WeakSet

  1. 成员都是对象
  2. 成员都是弱引用，可以被垃圾回收机制回收，可以用来保存DOM节点，不容易造成内存泄漏

- #### Map

  1. 本质上是键值对的集合，类似集合
  2. 可以根遍历，方法很多，可以跟各种数据格式转换

- #### WeakMap

  1. 只接受对象作为键名（null除外），不接受其他类型的值作为键名
  2. 键名是弱引用，键值可以使任意的，键名所指的对象可以被垃圾回收，此时键名是无效的
  3. 不能遍历，方法有`get`、`set`、`has`、`delete`

## Vue的性能优化方法有哪些

1. 路由懒加载 import方式引入页面

2. keep-alive缓存页面

3. 使用v-show复用DOM

4. v-for遍历的同时避免使用v-if（2.x 当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。3.x当 v-if 与 v-for 一起使用时，v-if 具有比 v-for 更高的优先级），可以使用计算属性提前把数组进行过滤

5. 长列表性能优化

   如果列表纯粹是显示数据，不会有改变，数据就不需要响应式

   ```vue
   export default{
   		data(){
   			return{
   				list: []
   			}
   		},
   		async created(){
   			const {data} = await apiGetList();
   			this.list = Object.freeze(data);
   		}
   }
   ```

   使用freeze方法进行冻结，或者更改属性为false

   如果是大数据列表，可以用虚拟滚动，只渲染少部分区域内容

6. 事件销毁，自定义的循环事件在`beforeDestroy`钩子函数中销毁

7. 图片懒加载，

8. 第三方插件按需导入

9. 无状态组件标记为函数组件

10. 子组件分割，子组件中有一些比较耗时的就单独分割成一个组件，自己做自己的渲染，不会影响其他的组件

11. SSR



## Vue响应式原理

### Object.defineProperty

Vue的响应式原理依赖于`Object.defineProperty()`，这里简单介绍一下

`Object.defineProperty()`会直接在对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。调用时直接在`Object`构造器对象上调用此方法，而不是在任意一个`Object`实例上调用。语法：

```javascript
/*
* obj: 要定义属性的对象
* prop: 要定义或修改的属性的名称或Symbol
* descriptor: 要定义或修改的属性描述符 
*				value:该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
*				writable:当且仅当该属性的 writable 键值为 true 时，属性的值，也就是上面的 value，才能被赋值运算符 (en-US)改变。
				configurable
当且仅当该属性的 configurable 键值为 true 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。
默认为 false。
				enumerable
当且仅当该属性的 enumerable 键值为 true 时，该属性才会出现在对象的枚举属性中。
默认为 false。
				get
属性的 getter 函数，如果没有 getter，则为 undefined。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。
默认为 undefined。
				set
属性的 setter 函数，如果没有 setter，则为 undefined。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 this 对象。
默认为 undefined。

默认为 false。
*/
Object.defineProperty(obj, prop, descriptor)
```

示例：

```JavaScript
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

object1.property1 = 77;
// throws an error in strict mode

console.log(object1.property1);
// expected output: 42
```



### Vue2是怎么实现数据双向绑定的

首先要对数据进行劫持监听，我们需要设置一个`Observer`函数，用来监听所有属性的变化。

如果属性发生了变化，就要告诉订阅者`watcher`看是否需要更新数据，如果订阅者有多个，则需要一个`Dep`来收集这些订阅者，然后在监听器`observer`和`watcher`之间统一管理。

还需要一个指令解析器`compile`，对需要监听的节点和属性进行扫描和解析。


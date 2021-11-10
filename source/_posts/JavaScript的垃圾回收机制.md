---
title: JavaScript之垃圾回收机制
cover: 'https://img.showydream.com/img/ZrtTjb-javascript.jpg'
description: 介绍一下JS的垃圾回收机制。
keywords: JS的垃圾回收机制
tags: Js笔记
categories:
  - JavaScript
abbrlink: 246bb01b
date: 2021-04-06 12:00:00
---



## 垃圾回收机制介绍

下面这段话引自《JavaScript权威指南（第四版）》：

>由于字符串、对象和数组没有固定大小，所以当它们大小已知时，才能对它们进行动态的存储分配。JavaScript程序每次创建字符串、对象或数组时，解释器都必须分配内存来存储那个实体。只要像这样动态地分配了内存。最终都要释放这些内存以便它们能够被再用，否则，JavaScript的解释器将会消耗完系统的内存，造成系统崩溃。

​		这段话解释了为什么需要系统垃圾回收，JS不详C语言或者C++，它有自己的一套垃圾回收机制（Garbage Collection）。JavaScript的解释器可以检测到何时程序不再使用一个对象了，当它确定了一个对象没用了以后，它就会把这个对象占用的内存释放掉。

​		JS的垃圾回收机制是为了防止内存泄漏，内存泄漏的含义就是当已经不需要某块内存时这块内存还存在着，造成内存的浪费。垃圾回收机制就是间歇的不定期的寻找到不再使用的变量，并释放掉它们所指向的内存。C#、Java、JavaScript有自动垃圾回收机制，但c++和c就没有垃圾回收机制，也许是因为垃圾回收机制必须由一种平台来实现。在JS中，JS的执行环境会负责管理代码执行过程中使用的内存。

​		不再使用的变量也就是生命周期结束的变量，是局部变量，局部变量只在函数的执行过程中存在，当函数运行结束，没有其他引用(闭包)，那么该变量会被标记回收。

全局变量的生命周期直至浏览器卸载页面才会结束，也就是说**全局变量不会被当成垃圾回收**。

### 内存的生命周期

1. 分配你所需要的内存：
   由于字符串、对象等没有固定的大小，js程序在每次创建字符串、对象的时候，程序都回分配内存来存储那个实体。

2. 使用分配到的内存做点什么

3. 不需要时将其释放回归：

   在不需要字符串、对象的时候，需要释放其所在的内存，否则将会消耗完系统中所有可用的内存，造成系统崩溃。这就是**垃圾回收机制所存在的意义**。

## 垃圾回收原理浅析

现在各大浏览器通常用采用的垃圾回收有两种方法：标记清除（mark and sweep）、引用计数(reference counting)。

1. ### 标记清除

   **工作原理**：

   ​		假定设置一个叫做根（root）的对象（在JavaScript里，根是全局对象）。垃圾回收器将定期从根开始，找所有从根开始引用的对象，然后找这些对象引用的对象……从根开始，垃圾回收器将找到所有可以获得的对象和收集所有不能获得的对象。

   ​		当变量进入环境时（例如局部声明一个变量），将这个变量标记为“进入环境”，当变量离开环境时，标记为“离开环境”。标记为“离开环境”的变量就会被回收内存。

   ```javascript
   function func3 () {
         const a = 1
         const b = 2
         // 函数执行时，a b 分别被标记 进入环境
   }
   func3() // 函数执行结束，a b 被标记 离开环境，被回收
   ```

   **工作流程**：

   1. 垃圾收集器会在运行的时候会给存储在内存中的**所有变量都加上标记**。
   2. **去掉环境中的变量**以及被环境中的变量引用的变量的标记。
   3. 那些**还存在标记的变量被视为准备删除的变量**。
   4. 最后垃圾收集器会执行最后一步内存清除的工作，**销毁那些带标记的值并回收它们所占用的内存空间**。

2. ### 引用计数

   统计引用类型变量声明后被引用的次数，当次数为 0 时，该变量将被回收

   ```javascript
   function func4 () {
         const c = {} // 引用类型变量 c的引用计数为 0
         let d = c // c 被 d 引用 c的引用计数为 1
         let e = c // c 被 e 引用 c的引用计数为 2
         d = {} // d 不再引用c c的引用计数减为 1
         e = null // e 不再引用 c c的引用计数减为 0 将被回收
   }
   ```

   但是引用计数的方式，有一个相对明显的缺点——循环引用

   ```javascript
   function func5 () {
         let f = {}
         let g = {}
         f.prop = g
         g.prop = f
         // 由于 f 和 g 互相引用，计数永远不可能为 0
   }
   ```

   解决方式是，当我们不使用它们的时候，手动切断链接：

   ```javascript
   f.prop = null
   g.prop = null
   ```

   **淘汰**：

   从2012年起，所有现代浏览器都使用了标记清除垃圾回收算法。所有对JavaScript垃圾回收算法的改进都是基于标记清除算法的改进。

## 哪些情况会引起内存泄漏？

1. #### 全局变量

   顾名思义，全局变量一直在全局环境中，所以它不会被垃圾回收机制标记“离开环境”。自定义的还好，但是有时候意外的全局变量就会导致内存泄漏。

   ```javascript
   function test(){
     name = '全局变量1'； // 没有声明的变量，实际上会指向window.name
     this.age = '全局变量2'； // this会向上找 如果找不到对象，就会指向window
   }
   test()
   ```

   **解决方法**：使用严格模式或者细心一点

   ```javascript
   function test(){
     "use strict";
     name = '全局变量1'； // 此时会直接报错
     this.age = '全局变量2'； // 严格模式下，this向上如果找不到对象，就会指向undefined
   }
   test()
   ```

   **手动释放变量**：

   ```javascript
   window.name = undefined
   delete window.age
   ```

2. #### 未销毁的定时器和回调函数

   ​		当**不需要**`setInterval`或者`setTimeout`时，**定时器没有被clear**，定时器的**回调函数以及内部依赖的变量都不能被回收**，造成内存泄漏。

   ```javascript
   var someResource = function(){
     return 'test'
   };
   setInterval(function() {
       var node = document.getElementById('test');
       if(node) {
           node.innerHTML = JSON.stringify(someResource));
       }
   }, 1000);
   ```

   如果后续 **test** 元素被移除, 整个定时器实际上没有任何作用. 但如果你没有回收定时器, 整个定时器依然有效, 不但定时器无法被内存回收, 定时器函数中的依赖也无法回收. 在这个案例中的 **someResource** 也无法被回收.

3. #### DOM引用

   ```html
   <body>
       <div id='test1'>213</div>
   </body>
   <script>
       var test1 = document.getElementById('test1');
       document.body.removeChild(test1); // dom删除了
       console.log(test1, "test1");  // 但是还存在引用
   </script>
   ```

   控制台输出：

   <img src="https://img.showydream.com/img/g92zJK-image-20210408115109640.png" alt="image-20210408115109640" style="zoom:50%;" />

   **解决办法**：test1 = null;

## 注意：闭包不会引起内存泄漏

老浏览器（主要是IE6）有bug，闭包会造成内存泄漏，这个现在已经无须考虑了。

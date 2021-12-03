---
title: JavaScript继承机制
cover: 'https://img.showydream.com/img/ZrtTjb-javascript.jpg'
description: JavaScript继承机制
keywords: JavaScript继承机制，prototype
tags: Js笔记
categories:
  - JavaScript
abbrlink: 5c6407b8
date: 2021-12-03 11:20:53
---



> JavaScript的继承做了两件事，一个是new一个构造函数的实例对象，一个是共享构造函数的prototype对象

JavaScript的设计者Brendan Eich为了解决实例对象之间无法共享属性和方法问题，新增了prototype属性，

这个属性是在构造函数里的，它包含一个对象（以下简称"prototype对象"），所有实例对象需要共享的属性和方法，都放在这个对象里面；那些不需要共享的属性和方法，就放在构造函数里面。

实例对象一旦创建，将自动引用prototype对象的属性和方法。也就是说，实例对象的属性和方法，分成两种，一种是本地的，另一种是引用的。

实例对象有一个`__proto__`属性，这个属性指向的是构造函数的prototype对象，这就构成了js的**原型链**

---
title: 面试问到HTTP就这么回答
cover: >-
  HTTPs://img.showydream.com/img/SoFF52-ryunosuke-kikuno-hyWy0z4UayE-unsplash.jpg
description: HTTP相关问答
tags: 面经
keywords: HTTP，HTTPS，面试题，
categories:
  - 前端
abbrlink: dc44fbe2
date: 2021-11-12 19:53:53
---

HTTP 协议是互联网的基础协议，也是网页开发的必备知识，最新版本 HTTP/2 更是让它成为技术热点。

## HTTP和HTTPs的区别

1. HTTP协议运行在TCP之上，所有传输的内容都是明文的，HTTPS运行在SSL/TLS之上，SSL/TLS运行在TCP之上，所有传输过程中的数据都是加密的
2. HTTPS协议需要到CA（数字证书 Certificate Authority 简称CA）申请证书
3. HTTP默认使用80端口号，HTTPS默认使用443端口
4. HTTPS用户访问速度较慢，需要[SSL握手](https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)，HTTPS 对速度会有一定程度的降低，而且服务器资源压力较大（要进行大量的加密算法计算，消耗CPU，内存），因此使用HTTPS需要做好足够的优化，
5. HTTPS可以有效的防止运营商劫持，解决了防劫持的一个大问题.

总结：HTTPS比HTTP安全

## 为什么HTTPS比HTTP安全?







当我们访问一个网站时，需要通过统一资源定位符（uniform resource locator，URL）来定位服务器并获取资源`<协议>://<域名>:<端口>/<路径>`







## 什么是强缓存和协商缓存



## 常见的HTTP状态码都代表了什么含义


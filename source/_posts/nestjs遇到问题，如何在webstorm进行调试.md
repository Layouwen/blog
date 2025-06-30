---
title: nestjs遇到问题，如何在webstorm进行调试
date: 2022-04-17 21:31
tags:
  - 博客
  - 后端
  - nestjs
  - nodejs
  - webstorm
categories:
  - 博客
  - 后端
  - nodejs
---

在开发过程中，经常需要log出来我们的数据查看。而像node程序终端log出来的数据不具备良好的可读性，所以能不能像浏览器调试一样阅读呢。

顶部菜单找到 `RUN` 接着选择 `Edit Configurations` 会弹出一个窗口

接着点击 `+` 号，选择 `Node.js` 接着参考下面图片，将 `Node parameters` 和 `Javascript file` 的参数修改为对应的数值。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0526367c91064e5c9a671123c4307a4f~tplv-k3u1fbpfcp-zoom-1.image)
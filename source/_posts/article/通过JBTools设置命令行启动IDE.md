---
title: 通过JBTools设置命令行启动IDE
date: 2022-02-14 11:13
tags:
  - 博客
  - 工具
  - jetbrain
categories:
  - 博客
  - 工具
  - jetbrain
---

## 需求

想通过类似vscode一样，可以通过在命令行输入 `code 目录` 直接启动 vscode 到对应目录。

想实现这个问题，只需要把对应的ide的快捷启动放置 `/usr/local/bin` 目录下。那么你在终端中可以直接识别到该执行程序。

## 设置终端启动

1. 打开 tools，点击设置

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc915a179fff42f4956c4a45b508f85a~tplv-k3u1fbpfcp-zoom-1.image)

2. 开启生成脚本，并指定生成目录

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88c9d48535e0430a845e07603511a822~tplv-k3u1fbpfcp-zoom-1.image)

> 如果你是 windows 可以生成后，先去环境变量确认是否已经自动添加了。确认后但还是没生效，只需要重启一下就可以了

3. 现在就可以在命令行进行使用

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af871f486a114393bfe66b5f39d30980~tplv-k3u1fbpfcp-zoom-1.image)
---
title: 解决Docker提示unsupported console settings, you just need to disable the legacy
date: 2022-06-29 22:09
tags:
  - 博客
  - docker
categories:
  - 博客
  - docker
---

第一步：`Win + R` 调出窗口

第二步：输入 `cmd` 回车，打开命令行

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/69c3b01aaeee41b489ae207df80a5506~tplv-k3u1fbpfcp-zoom-1.image)

第三步：在菜单栏右键点击 `Properties`

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/92b3ca09b5864254b0bed6d7e7711dfe~tplv-k3u1fbpfcp-zoom-1.image)

第四步：取消 `Use legacy console（requires relaunch. affects all consoles）`

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c33e5f97c3394618bb64e3b2b362fe6c~tplv-k3u1fbpfcp-zoom-1.image)

第五步：重启计算机，重新打开docker即可
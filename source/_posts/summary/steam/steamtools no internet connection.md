---
title: steamtools no internet connection
date: 2026-04-14T19:19:00.000Z
tags:
  - steam
categories:
  - steam
uuid: 9d5e8148-3771-402b-b719-9f1e984c8fc5
---

# 脚本方式

打开网站生成 steam manifest api key

[https://manifesthub1.filegear-sg.me/](https://manifesthub1.filegear-sg.me/)

管理员打开 powersheel
```bash
irm "https://luatools.vercel.app/manifests.ps1" | iex
```

在 steam要下载的游戏右键属性，点击 Updates 查看 App Id 填进去 powersheel 回车即可

# 手动方式

下载到的 manifests 清单压缩包，将里面 `.manifest` 后缀的文件手动复制到 `Steam\depotcache` 目录中

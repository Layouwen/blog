---
title: IDEA的ideaVim插件配置
date: 2020-08-30 15:35
tags:
  - 博客
  - idea
  - 前端
categories:
  - 博客
  - 前端
---

## 进入/创建ideaVim的配置文件

### windows

```
vim ~/_ideavimrc
```

```bash
:wq
```

### MocOS/Linux

```bash
vim ~/.ideavimrc
```

```bash
:wq
```

## 让ideaVim系统剪贴板与系统同步

在ideavimrc文件中添加一行
```bash
:set clipboard=unnamedplus,unnamed
```
保存退出
```bash
:wq
```
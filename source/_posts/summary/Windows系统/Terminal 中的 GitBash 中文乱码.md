---
title: Terminal 中的 GitBash 中文乱码
date: 2023-01-07 15:22:00
tags:
  - 汇总
  - windows
categories:
  - 汇总
  - Windows系统
---

# 打开 GitBash 设置 chartset

![](http://cdn.easyhappy.top/obsidian/20230123152200.png)

# 在 Terminal 中设置环境变量指定语言

```bash
vim ~/.bashrc
```

添加

```bash
export LANG='zh_CN.UTF-8'
```
---
title: 快速创建Vue项目
date: 2020-07-14 23:15
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
---

## 安装@vuecli

```bash
yarn global add @vue/cli
vue --version
```

创建项目

```bash
vue create demo
```

根据喜好配置，以下为我的设置

Manually select features
Babel,CSS Pre-processors,Linter/Formatter,Unit Testing
Lint and fix on commit
Jest

> 没有提及的为默认选项

## 使用codesandbox创建好的项目文件

进入 [官网](codesandbox.io) 接着创建一个vue项目，将项目导出来

## 运行

创建好vue项目文件后就可以开始本地预览了

```bash
yarn serve
```
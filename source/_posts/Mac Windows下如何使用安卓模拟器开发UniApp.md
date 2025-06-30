---
title: Mac/Windows下如何使用安卓模拟器开发UniApp
date: 2021-07-31 22:54
tags:
  - 博客
  - 前端
  - uniapp
categories:
  - 博客
  - 前端
---

## 1、Mac篇

### 1.1 网易MuMu

#### 1.1.1 下载并安装

[网易MuMu模拟器官网下载地址](https://mumu.163.com/mac/index.html)

#### 1.1.2 进入adb的目录

```bash
cd /Applications/NemuPlayer.app/Contents/MacOS
```

#### 1.1.3 连接模拟器的端口

```bash
./adb kill-server && ./adb server && ./adb shell
```

## 2、Windows篇

### 2.1 夜神

#### 2.1.1 下载并安装

[https://www.yeshen.com/](https://www.yeshen.com/)

#### 2.1.2 运行设置端口

运行夜神模拟器

打开终端，这里使用cmder来演示

进入根目录的 bin 目录（`Nox64/bin`）下，查看目前设备的端口

```bash
./nox_adb devices
```

连接对应端口

```bash
./nox_adb connect 127.0.0.1:你查看到的端口号
```

#### 2.1.3 打开HBuilderX，设置 adb 路径和端口

打开设置

adb路径：夜神模拟器根目录下，bin目录中的 **nox_adb.ext**

Android模拟器端口：设置你连接的端口号

例如：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/358cbd0aeb3b453eb72d2e1fa40ac471~tplv-k3u1fbpfcp-zoom-1.image)

#### 2.1.4 修复热更新

复制 HBuilderX 中的 adb 替换掉夜神模拟器中的 nox_adb

Hbuilder中的adb路径：`HBuilderX\plugins\launcher\tools\adbs`

复制一份后，修改名字为 nox_adb

接着替换掉原本的 nox_adb 即可
---
title: 使用charles进行小程序抓包
date: 2021-02-20 00:14
tags:
  - 博客
  - 前端
  - charles
  - 小程序
  - 抓包
categories:
  - 博客
  - 前端
  - 抓包
---

## 1、前言

因公司需求，需要使用uni进行平台同步开发。其中字节跳动的真机调试就很烦人。
ios无法进行真机调试，及时用安卓进入调试也只能看到发送请求。响应你就别想看了。
以至于你想看每个请求的响应数据，都要用console出来。所以引出来今天这篇文章。
如何使用charles进行小程序抓包。

## 2、下载

[charles下载地址](https://www.charlesproxy.com/latest-release/download.do)

## 3、安装

没什么好说的，无脑下一部即可。

Next => 勾选 I accept the terms is the License Agreement => Next => Next => Install => Finish

## 4、配置

### 4.1 前置准备

- 电脑打开了 **Charles** 软件
- 手机与电脑在同一个网段（你可以理解为用一个wifi）

接着我们只需要打开手机，进入wifi详情页。设置代理转发到ip地址的8888端口即可。

下面以 **Windows10** + **iphone11Pro** 举例

1. 在Windows打开命令行（Ctrl+R，输入cmd回车），输入`ipconfig`回车
2. 复制 `IPv4 Address` 的 `ip`地址
3. 打开手机，连接你的WiFi。点击wifi进入详情页，滑动到底部。看到有个代理， 打开它。然后输入我们复制的`IPv4 Address`的ip地址，以及端口号`8888`，点击保存
4. 手机点击保存后，如果你的输入正确，此时电脑会提示`Connection from 你的ip地址`的弹窗。我们点击`Allow`即可。

### 4.2 查看http包

完成上面的前置操作后，我们只需保证电脑的软件启动中。并且手机与电脑保持在一个网段。我们手机访问的http的包就会被 **Charles** 抓到。

### 4.3 查看https包

完成上面的操作后，你只可以查看http的包，如果是https的包会显示unknow。此时就继续往下操作。

1. 在 **Charles** 菜单栏找到 `Help`。点击 `SSL Proxying`,点击 `Install Charles Root Certificate on a Mobile Device or Remote Browser`。
2. 此时软件会弹出一个提示框，里面会显示你的端口号。`HTTP proxy on 你的ip地址:8888`,你可以核对一下你手机设置代理的时候是否与提示的ip地址一致。
3. 确保一致后，打开手机。进入浏览器，输入地址 `chls.pro/ssl`。点击允许 。然后点击关闭即可安装证书。如果你的IOS系统为10或以上，需要到 `设置` 页面，进行安装证书。
4. 在 **Charles** 菜单栏，选择 `Proxy`。选择 `SSL Proxying Settings`。在 `Include`处，点击 `Add`。Host和Port均输入 `*` 号。点击 `OK` 即可。
5. 大功告成，你现在可以用手机访问一下  `https` 协议的网站看看抓包效果了。

> 如果在本文章遇到什么问题，可以联系微信 `gdgzyw` 交流。
> 如果想交朋友，我也肥肠欢淫。

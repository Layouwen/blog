---
title: 简单了解 URL
date: 2020-05-11 11:27
tags:
  - 博客
  - 前端
  - url
categories:
  - 博客
  - 前端
---

## 一、URL

统一资源定位符（Uniform Resource Locator，缩写为 URL）
或称 统一资源定位器、定位地址、URL地址，俗称网页地址或简称网址，如同网络上的门牌定位到固定的地址。

### URL组成部分

一个标准的 URL地址，可包含以下组成部分
协议 + 域名 + 端口 + 资源层级路径 + 查询参数 + 锚点

如：
https://liangyouwen.com/index?name=wen#name
- https 协议部分
- liangyouwen.com 协议部分
- /index 资源层级部分，文件所在路径
- ?name=wen 查询参数部分，本地将此参数发送给服务器进行处理
- #name 锚点部分，不发送给服务器，用于浏览器解析，定位锚地

## 二、IP地址

ip地址（Internet Protocol Address，简称 IP Address，又或者 IP）
当设备连接网络，设备将被分配一个IP地址，用作标识。通过IP地址，设备间可以互相通讯，如果没有IP地址，我们将无法知道哪个设备是发送方，无法知道哪个是接收方。

### ping命令
可以使用 ping 命令，查看与该 IP地址 的连通情况。
在 命令行 或 终端 输入
格式为：ping IP地址/域名
如：
```cmd
ping www.baidu.com
```

## 三、DNS域名

域名系统（Domain Name System，缩写为 DNS）
是互联网的一项服务。它作为将域名和IP地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。


### 1. 域名作用

组要作用是用于将 IP地址，转换为更方便记忆的 名字。也就是 域名重定向，比如输入 baidu.com 或 qq.com 等。域名系统会将输入的域名解析为指定的 IP地址。

> 一个域名可以对应多个IP，称为 负载均衡
> 一个IP可以对应多个域名，称为 共享主机

### 2. nslookup命令

使用 nslookup 命令可以查看该域名所拥有的 IP地址。
在 命令行 或 终端 输入
格式为：nslookup 域名
如：
```cmd
nslookup baidu.com
```

### 3. 域名层次

域名分多个层次，从右往左依次递增。

如： baidu.com

- com 为顶级域名，顶级域名分为很多种：gov政府、net网络、edu教育机构、com商业等等
- baidu 为二级域名
- 依次递增

> 多数公司为了使自己网站容易记，都会使用 二级域名。
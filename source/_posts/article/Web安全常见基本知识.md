---
title: Web安全常见基本知识
date: 2022-03-08 22:18
tags:
  - 博客
  - web安全
categories:
  - 博客
  - web安全
---

## 1、XSS

跨站式脚本攻击。Cross-Site Scripting。因为与 CSS 重名所以变更为 XSS。

### 反射性

通过在传参处植入代码，实现数据的传输。

### 存储型

借助存储能力，植入恶意代码。当用户读取该输入时，如果是直接运行到页面。就会把恶意脚本一并执行。

### 常见危害

- 获取页面数据
- 获取Cookies
- 劫持前端逻辑
- 发送请求
- 偷取网页数据
- 偷取用户信息
- 偷取用户的登录态
- 欺骗用户

### 防范方案

#### HEAD

通过添加 header 来禁止 XSS 过滤

```http
ctx.set('X-XSS-Protection', 0)
```

> 0 禁止
> 1 启动（默认）

#### CSP

内容安全策略 (CSP, Content Security Policy)。建立白名单，告诉浏览器哪些外部资源可以加载和执行。

只允许加载本站资源

```http
Content-Security-Policy: default-src 'self'
```

只允许加载 HTTPS 协议图片

```http
Content-Security-Policy: img-src https://*
```

不允许加载任何来源框架

```http
Content-Security-Policy: child-src 'none'
```

#### 转义字符

#### 黑名单

将用户所以敏感字符串，全部替换成转义字符。

安装 xss 依赖。使用 `xss('<script>alert('layouwen')</script>')` 函数转义。

#### HttpOnly Cookie

设置请求头，不允许 js 直接获取 cookie。防止 XSS 攻击后获取信息。

```http
response.addHeader('Set-Cookie', 'name=layouwen; Path=/; HttpOnly')
```

## 2、CSRF

CSRF(Cross Site Request Forgery)，跨站请求伪造。

通过特殊方式，诱导我们在恶意网址中请求我们目标地址。会把我们的 cookie 一并携带。如果 hack 对我们请求参数做了手脚，则会伪造身份进行操作。

### 常见危害

- 利用用户登录态
- 伪造业务请求
- 冒充用户发帖

### 防范方案

1. 禁止第三方网站带 Cookie
2. Referer Check

检查请求方，是否是我们白名单
 
3. 短信 / 邮箱 / 滑动验证码

## 3、clickjacking

点击劫持，通过嵌入iframe并通过某种方式隐藏。欺骗用户点击按钮发送信息。

### 防范方案

#### X-FRAME-OPTIONS

设置请求头，不允许 iframe 嵌入。

- DENY 不允许
- SAMEORIGIN 同域名允许
- ALLOW-FROM 允许指定来源

```http
ctx.set('X-FRAME-OPTIONS', 'DENY')
```

#### js判断

通过 js 脚本，判断网站是否被嵌套。如果嵌套了就把内容清空隐藏。

## 4、SQL注入

通过组装条件，使连接 sql 查询时。满足不应该满足的条件，从而获取数据。

### 防御方案

使用库，将 sql 语句进行转义并处理后执行。

## 5、OS命令注入

与 SQL 注入类似，都是组装命令从而执行恶意内容。

## 6、请求劫持

### DNS劫持

在 DNS 解析的时候，引导用户访问错误的结果。

### HTTP劫持

这个最简单的方法是升级 https

## 7、常见攻击方式

### SYN Flood

我们都知道 TCP 有三次握手。通过大量的连接，每次连接都不完成三次，时TCP连接处于等待下一次握手的状态。达到耗尽目标资源。

### HTTP Flood

通过大量的访问，导致服务器处理不过来瘫痪。
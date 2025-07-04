---
title: npm 知识点
date: 2023-01-23 15:22:00
tags:
  - 汇总
  - npm
categories:
  - 汇总
  - npm
---

# 防止意外发布代码

`package.json` 中 `"private": true` 可以防止意外发布代码 `npm public`。主要应用于一些业务代码，如前端、后端页面代码。

# 指定仓库源

> packages.aliyun.com/6247b3d254d03ae1449d/npm/npm-registry/ 是仓库源地址

```bash
registry=https://packages.aliyun.com/6247b3d254d03ae1449d/npm/npm-registry/
```

# token

## 创建token

```bash
npm create token
```

## 使用token授权

```bash
//packages.aliyun.com/6247b3d254d03ae1449d/npm/npm-registry/:_authToken=b7a28d1d-f7f3-4383-b896-3e92616cd
```

## 通过token进行login

create token

```bash
$ nrm add 源名字 源地址
$ nrm use 源名字
$ npm login
用户名
密码
邮箱
$ npm token create
密码
```

use token `.nrmrc`

```
//源域名/:_authToken=你创建的token
registry = "源地址/"
```

# 单独为某些依赖设置安装源

example：以@avan开头的依赖包，走https://npm.avan.com

```bash
$ npm config set @avan:registry=https://npm.avan.com
```

# 新建用户

```bash
$ npm adduser --registry 需要注册的地址
$ 账号名
$ 密码
$ 邮箱
```
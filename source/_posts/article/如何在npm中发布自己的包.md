---
title: 如何在npm中发布自己的包
date: 2020-07-14 07:46
tags:
  - 博客
  - 前端
  - npm
categories:
  - 博客
  - 前端
---

## 注册账号

[npmjs官网](https://www.npmjs.com/)

注册完成后保持登录状态，不要关闭网页。

## 设置源

因为下载问题，你可能将npm的官方源切换为淘宝源。这里需要重新设置为官方源才能提交成功，使用nrm工具快速切换。

```bash
npm i -g nrm
nrm use npm
```

> 这部分设置后，你使用npm安装速度会变慢。最后发包完成后在切换回去

```
npm use taobao
```

## 初始化包

```bash
npm init
```

根据提示输入对应内容。

> package name => 你要发布的包名
> version => 当前发布的版本号（如果你是第一次发布建议设置为0.0.1）
> description => 对你的包进行描述
> keywords => 关于你包的关键字
> author => 作者名（填写你注册的用户名）

其他没提及的默认回车即可。

## 发布包

添加你的发布账号

```bash
npm adduser
```

接着输入你的账号及密码。（这里也是需要将源切换为官方的npm源才可以成功）

添加结束后即可，发布你的一个包。

```bash
npm publish
```

> 后续发布可以到package.json文件中修改 "version" 的版本号在提交。
> 如果同一版本提交会报错
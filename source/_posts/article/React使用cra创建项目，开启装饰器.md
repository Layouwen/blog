---
title: React使用cra创建项目，开启装饰器
date: 2021-08-19 15:54
tags:
  - 博客
  - 前端
  - react
categories:
  - 博客
  - 前端
---

## 前言

今天使用 cra 创建了个项目。打算研究一下 rc-form 的实现思路。发现项目并未开启装饰器。并提示下面的错误。

`Support for the experimental syntax 'decorators-legacy' isn't currently enable`

## 解决方案

安装对应的依赖库

```bash
yarn add @babel/plugin-proposal-decorators customize-cra react-app-rewired
```

接着配置一下根目录 **config-overrides.js** 文件

```js
const { override, addDecoratorsLegacy } = require('customize-cra')
module.exports = override(addDecoratorsLegacy())
```

接着修改 **package.json** 的启动参数

```json
"scripts": {
  "start": "react-app-rewired start"
},
```

重新运行，大功告成
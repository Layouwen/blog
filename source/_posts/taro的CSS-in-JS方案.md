---
title: taro的CSS-in-JS方案
date: 2020-09-01 21:42
tags:
  - 博客
  - taro
  - 前端
  - css-in-js
categories:
  - 博客
  - 前端
---

## linaria

在taro中使用React开发，无法使用之前的styled-components的CSS方案。官方提供了一个linaria的一种CSS样式方案。这种方案与styled-components方案类似。

### 安装及使用

1.下载安装

```bash
yarn add linaria
```

2. 配置 `babel.config.js`

```js
module.exports = {
  presets: [
    ['taro', {
      framework: 'react',
      ts: true
    }],
    'linaria/babel' // 添加到 babel-presets
  ]
}
```

3. 配置 `config/index.js`

```js
const config = {
  mini: {
    webpackChain(chain, webpack) { // 添加到config-mini
      chain.module
        .rule('script')
        .use('linariaLoader')
        .loader('linaria/loader')
        .options({
          sourceMap: process.env.NODE_ENV !== 'production',
        })
    }
  }
}
```

4. 新建文件 `linaria.config.js`

```js
module.exports = {
    ignore: /node_modules[\/\\](?!@tarojs[\/\\]components)/,
}
```

5. 使用方式

```jsx
import React from "react"
import { View } from "@tarojs/components"
import { styled } from "linaria/react"

const Title = styled(View)`
    color: #333;
    background: red;
`

function Index(){
    return (
        <Title>
            Hello World!
        </Title>
    )
}
```
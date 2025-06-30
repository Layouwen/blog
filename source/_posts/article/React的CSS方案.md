---
title: React的CSS方案
date: 2020-09-01 16:23
tags:
  - 博客
  - react
  - 前端
  - css-in-js
categories:
  - 博客
  - 前端
---

## 一、方案选择

React的css in js有很多种，这里推荐按照start的数量进行选择。如果有一些特别合自己心意的除外。以下是一个搜集方案的网址

[点我跳转](https://github.com/MicheleBertoli/css-in-js)

## 二、使用示例

### styled-components

[官方文档](https://styled-components.com/)

1. 安装

```bash
yarn add styled-components
```

2. 导入

```jsx
import styled from 'styled-components'
```

3. 定义标签

```jsx
const Div1 = styled.div`
    width: 100px;
    height: 100px;
    border: 1px soild red;
`
```

4. 直接使用

```jsx
return (
    <div>
        我是一个styled标签
        <Div1 />
    </div>
)
```

### emotion

[官方文档](https://emotion.sh/docs/introduction)

1. 安装

```bash
yarn add @emotion/core
```

2. 导入

```jsx
/** @jsx jsx */
import {jsx} from "@emotion/core"
```

3. 直接在jsx中使用css属性使用

```jsx
return (
    <div css={{ border: 1px soild red, width: 100, height: 100 }}>我是emotion方案</div>
)
```
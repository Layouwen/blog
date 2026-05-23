---
title: css 知识点
date: 2022-03-22T23:11:34.000Z
tags:
  - 汇总
  - css
categories:
  - 汇总
uuid: 94d43e54-7ad4-4d2c-a0f5-e904989fb84c
---

# CssModule

在 modules 中某个 class 不需要随机数

```scss
:global(.blue) {
  background: red;
}
```

# :where 伪类

降低优先级为 0

```css
:where(.name) {
  color: red;
}
```

# 水平和垂直滚动条交汇处样式调整

```css
::--webkit-scrollbar-corner {
  background: transparent;
}
```

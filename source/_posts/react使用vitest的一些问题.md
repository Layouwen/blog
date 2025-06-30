---
title: react使用vitest的一些问题
date: 2022-01-08 17:59
tags:
  - 博客
  - 前端
  - react
  - vitest
categories:
  - 博客
  - 前端
---

## 1、TypeError: __vite_ssr_import_0__.default is not a function

为项目中使用到了classnames，但是他提示不是个函数

### 解决方法

安装classnames-es-ts

```bash
yarn add -D classnames-es-ts
```

配置vite别名

```ts
export default defineConfig({
  ...
  resolve: {
    alias: {
      classnames: "classnames-es-ts"
    },
  }
});
```

> 如果还是不行，尝试降低一下版本号  
> "vitest": "0.0.131"
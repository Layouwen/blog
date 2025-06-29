---
title: Nuxt3 中使用 pnpm 安装的 NuxtImg 使用会提示找不到图片
date: 2025-06-29 17:18:54
tags:
  - nuxt
  - 前端
categories:
  - 博客
  - 前端
---

![image.png](https://qinius.easyhappy.top/avan/202506111712538.png)


## **背景**

在一个全新的 Nuxt 3 项目中，我打算像往常一样直接写：


```vue
<template>
  <NuxtImg src="/images/logo.png" />
</template>
```

然而页面报错：

```bash
http://localhost:3000/_ipx/_/images/logo.png 500 (Internal Server Error)
```

猜测这一错误常见于 **IPX** （Nuxt 默认的本地图片处理服务）无法正常工作或缺少二进制依赖，如 sharp 等。

## **问题复现与初步排查**

1. **确认官方文档**
    
    Nuxt Image 文档并未提到需要额外配置即可本地使用。
    
2. **定位到 IPX**
    
    查看 Nuxt Image 的默认 provider，可见如果未显式配置，Nuxt 会自动启用 IPX。
    
3. **搜索社区反馈**
    
    多位开发者在 GitHub 上反馈升级或安装时出现「IPX 500」或「sharp 模块缺失」问题，且大多与包版本或二进制构建方式有关。
    

## **解决方案**

### **步骤一：锁定 IPX 版本**

在 package.json 中新增（或合并）以下字段，强制 pnpm 使用 ipx@^3.0.0：

```json
// package.json
{
  "pnpm": {
    "overrides": {
      "ipx": "^3.0.0"
    }
  }
}
```

内心OS: 估计是版本 3 已默认内置预编译好的 sharp 二进制，避免了跨平台自行编译失败的问题。

### **步骤二：重新安装依赖并启动**

```bash
pnpm install
pnpm dev
```

此时刷新页面，/_ipx/_ 路径应能正确返回处理后的图片。  

## **为什么 pnpm 会「漏装」或装错 IPX？**

- **严格的依赖隔离**：pnpm 以硬链接 + 独立虚拟 store 机制保存依赖，若某深层模块声明的版本范围与项目锁文件冲突，可能被解析为旧版。
- **可选依赖**：IPX 及其 sharp 本身属于可选依赖；当安装时遇到编译失败，pnpm 会跳过而不抛错，导致运行期才暴露 500。
- **overrides 的作用**：显式声明 overrides 可让 pnpm「顶置」该版本，强制一致，避免多版本并存。

## **完整代码示例**

```json
// package.json（节选）
{
  "name": "nuxt-img-demo",
  "dependencies": {
    "@nuxt/image": "^1.3.0"
  },
  "pnpm": {
    "overrides": {
      "ipx": "^3.0.0"
    }
  }
}
```

```vue
<!-- pages/index.vue -->
<template>
  <NuxtImg src="/images/logo.png" width="200" height="200" placeholder />
</template>
```

## **小结**

- Nuxt 3 里 <NuxtImg> 默认依赖 IPX；IPX 依赖 sharp。 
- 当 IPX 版本不兼容或二进制缺失时，会抛出「500 – IPX Error」。  
- 在 **pnpm** 项目中，可通过 pnpm.overrides 强制锁定 ipx@^3.0.0，再重新安装即可快速修复。
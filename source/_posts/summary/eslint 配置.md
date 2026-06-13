---
uuid: f1e580af-84c6-4b97-961a-986dd3edd5d5
title: eslint 配置
date: 2020-02-12 16:40
categories:
  - 汇总
tags:
  - eslint
  - 汇总
---

安装 eslint

```bash
pnpm i -D @antfu/eslint-config eslint@9.x.x
```

配置默认值

``` ts
  const {
    angular: enableAngular = false,
    astro: enableAstro = false,
    autoRenamePlugins = true,
    componentExts = [],
    e18e: enableE18e = true,
    gitignore: enableGitignore = true,
    ignores: userIgnores = [],
    imports: enableImports = true,
    jsdoc: enableJsdoc = true,
    jsx: enableJsx = true,
    nextjs: enableNextjs = false,
    node: enableNode = true,
    perfectionist: enablePerfectionist = true,
    pnpm: enableCatalogs = !!findUpSync('pnpm-workspace.yaml'),
    react: enableReact = false,
    regexp: enableRegexp = true,
    solid: enableSolid = false,
    svelte: enableSvelte = false,
    type: appType = 'app',
    typescript: enableTypeScript = isPackageExists('typescript') || isPackageExists('@typescript/native-preview'),
    unicorn: enableUnicorn = true,
    unocss: enableUnoCSS = false,
    vue: enableVue = VuePackages.some(i => isPackageExists(i)),
  } = options
```

安装 prettier 格式化普通文件

```bash
pnpm i -D eslint-plugin-format
```

eslint.config.mjs

```ts
import antfu from '@antfu/eslint-config';

export default antfu({
  stylistic: {
    semi: true,
  },
  // react: true, // 手动开启
  formatters: true,
});
```

react 安装

```bash
npm i -D @eslint-react/eslint-plugin eslint-plugin-react-refresh
```

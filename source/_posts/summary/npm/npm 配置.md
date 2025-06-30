---
title: npm 配置
date: 2023-01-23 15:22:00
tags:
  - 汇总
  - npm
categories:
  - 汇总
  - npm
---

# 取消动态版本号

```bash
# 取消新安装的包的 ^ 前缀
npm config set -g save-prefix=''
pnpm config set -g save-prefix=''
yarn config set save-prefix ""
```
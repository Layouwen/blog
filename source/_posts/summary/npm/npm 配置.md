---
title: npm 配置
date: 2023-01-23T15:22:00.000Z
tags:
  - 汇总
  - npm
categories:
  - 汇总
  - npm
uuid: ae22e575-d217-4f78-8de9-d690515dda3b
---

# 取消动态版本号

```bash
# 取消新安装的包的 ^ 前缀
npm config set -g save-prefix=''
pnpm config set -g save-prefix=''
yarn config set save-prefix ""
```

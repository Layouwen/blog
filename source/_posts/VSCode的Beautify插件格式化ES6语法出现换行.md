---
title: VSCode的Beautify插件格式化ES6语法出现换行
date: 2020-08-17 11:08
tags:
  - 博客
  - vscode
  - 前端
categories:
  - 博客
  - 前端
---

在settings(JSON)中添加此配置即刻，之前换行的需要手动删除回车

```json
"beautify.config": {
  "brace_style": "collapse,preserve-inline"
}
```
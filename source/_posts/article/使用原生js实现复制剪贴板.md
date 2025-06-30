---
title: 使用原生js实现复制剪贴板
date: 2021-12-28 09:12
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

在平时需求中，经常会遇到一些秘钥，或者一大串文章需要复制再或者百度网盘链接。此时我们可以提供一个按钮，让用户一键复制

```javascript
const el = document.createElement('input');
el.setAttribute('value', secret);
document.body.appendChild(el);
el.select();
const flag = document.execCommand('copy');
document.body.removeChild(el);
flag && alert('复制成功');
```

这段代码，在不同框架放到，事件监听内即可。提示方式也可以用相应的组件进行替代。

简单解释一下思路：  
- 创建一个input标签
- 把我们需要赋值的内容设置到属性中
- 调用选择方法，选中文本
- 接着执行浏览器的复制命令
- 复制成功后删除我们临时生成的节点
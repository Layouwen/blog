---
title: Webpack loader 和 Webpack plugin 区别
date: 2020-03-17 23:08
tags:
  - 博客
  - 前端
  - webpack
categories:
  - 博客
  - 前端
---

+ 思路
+ 回答



## 一、思路
<font style="color:#333333;">所有问两个的区别都是，英文翻译成中文+对翻译的解释+举例子</font>

<font style="color:#333333;"></font>

## 二、回答
### - 将英文翻译
loader是一个加载器

plugin是一个插件



### - 对翻译解释
1. loader 加载器 是用来用来load一个个文件的，比如说：  
babel loader 用来加载高级的js，变成低版本浏览器支持的js文件  
style loader 和 css loader 是用来加载 css，变成页面中style标签  
还可以加载图片文件，对图片文件进行一些优化



2. 插件 是用来加强功能，比如说：  
HtmlWebpackPlugin 用来单独自动生成 html页面，这个插件可以自己指定一个模板，根据模板内容来进行生成  
MiniCssExtractPlugin 用来将loader加载器 加载的style标签单独提取出来，变成一个css文件


---
title: 快速了解HTML语言
date: 2020-02-08 01:18:16
tags:
  - html
  - 前端
categories:
  - 博客
  - 前端
---

# 一、本文章食用须知
软件准备：IDE代码编辑器，如：VScode、Sublime Text等

VScode [链接](https://code.visualstudio.com/)

Sublime Text [链接](https://www.sublimetext.com/)

本文章适用于没学过HTML语言的，或者刚接触不久想更加了解的人群。



# 二、HTML的发明者
Tim Berners-Lee（提姆·柏內茲-李）1955年6月8日-至今。

详情请看维基百科 [链接](https://zh.wikipedia.org/wiki/%E8%92%82%E5%A7%86%C2%B7%E4%BC%AF%E7%BA%B3%E6%96%AF-%E6%9D%8E)

李爵士做了些什么事情：

1. 编写了第一个浏览器
2. 编写了第一个服务器
3. 使用自己编写的浏览器访问自己编写的服务器
4. 发明了WWW，以及HTML、HTTP、URL

做这些事情为了什么：让每个人输入网址就能看到网页

# 三、HTML起手代码
如果你安装的是VScode，只需要在VScode里面创建html后缀名的文件，输入 ! 然后按TAB键即可自动生成下列内容：



```html
<!DOCTYPE html>    <!-- 声明是html5文本类型 -->
<html lang="en">    <!-- 使用的语言：en是英语 zh-CN是中文 -->
<head>    <!-- 头部标签开始 --> 
	<meta charset="UTF-8" />    <!-- 编码格式 -->
	<meta name="viewport" content="width=device-width, initial-scale=1.0" /><!-- 防止缩放 -->
	<meta http-equiv="X-UA-Compatible" content="ie=edge" /><!-- 检查是否IE浏览器，自动调至最新 -->
	<title>Document</title>    <!-- 网站的标题 -->
</head>    <!-- 头部标签结束 -->
<body>    <!-- body标签开始，页面的代码基本上都写在里面-->
	内容
	...
</body>
</html>
```

如果你是别的编辑器，或者没有这个插件，请手动输入上面内容。

# 四、常用的章节标签
### 1、标题标签

```html
<h1></h1>
<h2></h2>
<h3></h3>
<h4></h4>
<h5></h5>
<h6></h6>
```

### 2、章节标签

```html
<section></section>
```

### 3、文章标签

```html
<article></article>
```

### 4、段落标签

```html
<p></p>
```



### 5、头部标签

```html
<header></header>
```

### 6、脚部标签

```html
<footer></footer>
```



### 7、主要内容标签

```html
<main></main>
```

### 8、旁支标签

```html
<aside></aside>
```



以上都是一些比较常用的章节标签。



# 五、全局属性
class、contenteditable、hidden、id、style、tabindex、title等。



# 六、常用的内容标签
### 1、有序标签

```html
<ol>
  <li></li>
  <li></li>
  <li></li>
  ...
</ol>
```



### 2、无序标签


```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    ...
</ul>
```

### 
```html
<dl>
    <dt></dt>
    <dd></dd>
    <dd></dd>
    <dt></dt>
    <dd></dd>
    <dd></dd>
    ...
</dl>
```



### 4、链接标签


```html
<a href=""></a>
```



### 5、代码标注标签


```html
<code></code>
```


### 6、脚部标签


```html
<footer></footer>
```



### 7、水平分割线标签


```html
<hr>
```


### 8、换行标签


```html
<br>
```



以上都是一些比较常用的内容标签。

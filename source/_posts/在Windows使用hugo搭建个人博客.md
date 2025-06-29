---
title: 在Windows使用hugo搭建个人博客
date: 2020-02-07 22:02:16
tags:
  - hugo
categories:
  - 博客
---

<!-- more -->

## 一、本文章食用须知
1. 能访问外网
2. 有自己的github账号以及配置了本地git的服务
3. 对终端操作有所了解
4. 仔细看完每个字

如果以上都有了解，就可以开始下面的搭建博客啦~

## 二、下载hugo安装包
链接：[Hugo官网](https://gohugo.io/)、[安装包下载地址](https://github.com/gohugoio/hugo/releases)

### Windows用户
找到hugo_版本号_Windows-你的系统版本bit.zip下载下来

这里以hugo0.64版本为例：

我的电脑是Windows10-64位系统

就下载hugo_0.64.0_Windows-64bit.zip

### Mac用户
```bash
brew install hugo
hugo version
```

## 三、解压安装包
解压到你所需要放的目录。如：D:\Programme\hugo_0.64.0_Windows-64bit

配置hugo环境变量

复制你刚刚解压位置的路径，添加到环境变量中，这里以Win10为例。

此电脑【右击】 --> 选择 属性 --> 高级系统设置 --> 环境变量 --> 系统变量 --> Path --> 编辑 --> 新建 --> 粘贴你的路径D:\Programme\hugo_0.64.0_Windows-64bit --> 确定

![](http://obsidian.easyhappy.top/avan/202506291645555.png)

打开终端段输入

```bash
hugo version
```

 

如果有提示Hugo字样则配置成功。



## 四、开始配置博客
回到终端，cd到你需要存放的目录位置，输入以下指令

```bash
hugo new site github用户名小写.github.io-creator
```

输入完后会在当前目录创建一个“github用户名小写.github.io-creator”的文件夹，cd进去，输入指令



```git
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```



输入完后它会下载一些东西，等它下完。下完后接着输入

```bash
echo 'theme = "ananke"' >> config.toml
```

输入完后即可创建你的一篇博客，内容可以先不写。创建指令

```bash
hugo new posts/第一篇博客的名称.md
```

输入完后在你目录进入 content 文件夹，在进入 posts 文件夹，即可看到你刚刚创建的博客文章。编辑这个文件，将里面的“true”修改为“false”

![](http://obsidian.easyhappy.top/avan/202506291645307.png)

保存关闭，回到终端。在根目录也就是“github用户名小写.github.io-creator”的目录，输入指令

```bash
hugo server -D
```

输入完后可以先不要操作，在浏览器输入 localhost:1313 可以预览本地博客。



## 五、将本地博客上传到github
在github新建一个仓库，名字为“用户名.github.io”，然后回到终端进入 public目录，将目录的文件上传到该仓库。下面以我的用户名为例：

![image.png](http://obsidian.easyhappy.top/avan/202506291646140.png)

输入完后点下面绿色的按钮就好了。我的创建过所以提示红色，你们直接点就好。

点完后会跳转到另一个界面，点击SSH,然后复制下面的指令，回终端public目录输入即可

![image.png](http://obsidian.easyhappy.top/avan/202506291646720.png)

紧接着就是git的一些基础操作，回终端将public里的文件提交到这个仓库。下面提供一些基础指令做参考，具体去了解git的使用教程：

```git
git init
git add .
git status
git commit -v
```





提交完后，回到github在刚刚仓库上面点击Settings

![image.png](http://obsidian.easyhappy.top/avan/202506291646281.png)

往下拉找到GitHub Pages，下面显示的就是你的博客域名了，有买私人域名的可以自己绑定，这里就不说了。开始你的博客人生吧~

![image.png](http://obsidian.easyhappy.top/avan/202506291647827.png)


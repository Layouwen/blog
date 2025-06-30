---
title: VScode的Git本地仓库的配置
date: 2020-02-12 16:56
tags:
  - 博客
  - vscode
  - git
categories:
  - 博客
  - vscode
---

+ 前置条件
+ 配置命令
+ 一些常用指令



## 一、前置条件


1. 安装了VScode，并且配配置了环境变量
2. 安装了git环境



## 二、配置指令


```git
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```



如：



```git
git config --global user.name Layouwen
git config --global user.email Layouwen@gmail.com
git config --global push.default simple
git config --global core.quotepath false
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```



以上六条命令运行后，输入 `git config --global --list` 查看配置有没有输错。  
![image.png](http://obsidian.easyhappy.top/avan/202506291903853.png)




配置好后，就可以使用git的指令了



## 
## 三、常用指令


```git
// 初始化创建一个.git目录
git inte  

// 添加需要备份的文件
git add .  

// 开始备份
git commit -m "备注"  

// 查看备份记录
git log

// 回滚之前版本
git reset --hard 版本号
```



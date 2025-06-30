---
title: 简单配置VScode及常用快捷键
date: 2020-02-12 16:34
tags:
  - 博客
  - vscode
categories:
  - 博客
  - vscode
---

# 下载VScode


官网链接：[https://code.visualstudio.com/](https://code.visualstudio.com/)



# 安装VScode


无脑下一部就好了  
中有个步骤需要选择，如果看的懂就自己选，如果看不懂，就全选



# 配置VScode


一、汉化  
使用快捷键 `Ctrl + Shift + X` 打开拓展菜单，搜索框输入 Chinese，找到自己需要的语言点击Install安装，安装完重启即可  
![image.png](http://obsidian.easyhappy.top/avan/202506291707770.png)


  
二、配置



1. 调整字体  
文件——首选项——设置，搜索设置输入“字体”  
在 `Font Size` 属性修改字体大小  
![image.png](http://obsidian.easyhappy.top/avan/202506291707328.png)




2. 自动保存  
搜索设置输入“auto save”  
将 `Auto Save` 属性的值修改为 `onFocusChange`  
![image.png](http://obsidian.easyhappy.top/avan/202506291708891.png)




3.自动格式化（适合新手，会格式化他人的代码，慎用）  
搜索设置输入“format on save”  
勾选 `Format On Save` 属性的选项  
![image.png](http://obsidian.easyhappy.top/avan/202506291708460.png)




# 可选项
## 设置fira code字体和设置字体连字


1. 下载字体安装包 [字体github链接](https://github-production-release-asset-2e65be.s3.amazonaws.com/26500787/88777d80-a7f1-11e9-95ae-146629eb2946?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20200210%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200210T070105Z&X-Amz-Expires=300&X-Amz-Signature=a6c29f774fc63b8dd76f2e2ab74fa657fe1f160da08d25b6daecc3b98876ec2f&X-Amz-SignedHeaders=host&actor_id=60692794&response-content-disposition=attachment%3B%20filename%3DFiraCode_1.207.zip&response-content-type=application%2Foctet-stream)
2. 解压到电脑，进入 otf 文件夹，`Ctrl + A`全选，右击安装  

![image.png](http://obsidian.easyhappy.top/avan/202506291708422.png)




3. 进入首选项，搜索设置输入“字体”  
在属性前面输入 “fira font, ” 注意有个逗号，或者直接复制我的

`Fira Code,Consolas, 'Courier New', monospace`  
    点击 `Font Ligatures` 属性下的编辑，进入json文件，在中键位置复制粘贴这行指令

`"editor.fontLigatures": true,` 然后 `Ctrl + S` 保存退出  
![image.png](http://obsidian.easyhappy.top/avan/202506291710637.png)


  
![image.png](http://obsidian.easyhappy.top/avan/202506291711331.png)




# VScode插件


1. Code Spell Checker（检查有没有拼错单词）  
使用快捷键 `Ctrl + Shift + X` 打开拓展菜单，搜索框输入 “Code Spell Checker”，点击 Install 安装即可  
![image.png](http://obsidian.easyhappy.top/avan/202506291711762.png)




2. Git Easy（方便git操作）  
使用快捷键 `Ctrl + Shift + X` 打开拓展菜单，搜索框输入 “Git Easy”，点击 Install 安装即可



# Vscode简单的快捷键


`Ctrl + P`  找文件

`Ctrl + Shift + P`  输入命令

`Alt + 鼠标单击`  多位置输入


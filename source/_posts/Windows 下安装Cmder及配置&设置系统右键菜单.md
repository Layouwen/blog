---
title: Windows 下安装Cmder及配置&设置系统右键菜单
date: 2020-02-12 16:40
tags:
  - 博客
  - windows
  - cmder
categories:
  - 博客
  - windows
---

# 下载Cmder


Cmder的官网 [https://cmder.net/](https://cmder.net/)  
Cmder的GitHub下载链接 [点击这里](https://github-production-release-asset-2e65be.s3.amazonaws.com/11276147/eb0e7b00-3262-11ea-8f83-c17cae9b8b2c?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20200210%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20200210T075928Z&X-Amz-Expires=300&X-Amz-Signature=9a1d570c489bd1abcc9dd2ceb57e9f965ca8e7adecd4a8f795332c2c035f68e8&X-Amz-SignedHeaders=host&actor_id=60692794&response-content-disposition=attachment%3B%20filename%3Dcmder.zip&response-content-type=application%2Foctet-stream)



# 安装Cmder


直接解压到文件夹下，打开目录下的 Cmder.exe 即可运行  
![image.png](https://qinius.easyhappy.top/avan/202506291713700.png)




# 简单的配置Cmder


1. 在右下角 鼠标右键 打开设置  
![image.png](https://qinius.easyhappy.top/avan/202506291713456.png)

2. 设置语言为中文  
![image.png](https://qinius.easyhappy.top/avan/202506291714820.png)

3. 把这三个打上勾  
![image.png](https://qinius.easyhappy.top/avan/202506291714823.png)

4. 在设置左边点击 大小&位置 ，按下方图片的数据来设置（可以自己设置）  
![image.png](https://qinius.easyhappy.top/avan/202506291714801.png)

5. 单击保存设置  
![image.png](https://qinius.easyhappy.top/avan/202506291714047.png)




PS：如果发现下次打开设置页面，又恢复成中文则进行一下操作



1. 再次打开设置，点击导出，右键 ConEmu.xml 文件点击编辑  
![image.png](https://qinius.easyhappy.top/avan/202506291715652.png)

2. 在新打开的记事本中，按 `Ctrl + F` 搜素关键字 “Language”  
![image.png](https://qinius.easyhappy.top/avan/202506291715179.png)

3. 将后面的 en 改为 zh 然后按 `Ctrl + S` 保存退出即可。  
![image.png](https://qinius.easyhappy.top/avan/202506291715659.png)




# 设置鼠标移出后隐藏Cmder


在设置页面，单击右边的 Quake 风格 然后勾选下方图片的三个选项，保存设置  
![image.png](https://qinius.easyhappy.top/avan/202506291715076.png)




# 修改呼出快捷键


在设置页面，单击右边的 通用 然后按下方图片位置，自定义快捷键，我是 `Alt + 1`  
![image.png](https://qinius.easyhappy.top/avan/202506291715566.png)




# 设置默认bash启动


在设置页面，单击右边的 启动 然后按下方图片位置进行设置  
![image.png](https://qinius.easyhappy.top/avan/202506291716676.png)




# 设置快捷键


在设置页面，单击右边的 按键&宏 即可自定义快捷键  
![image.png](https://qinius.easyhappy.top/avan/202506291716515.png)




# 设置bash启动目录


在设计页面。单击右边的 启动，单击 任务 ，然后进行下面图片的设置即可  
![image.png](https://qinius.easyhappy.top/avan/202506291716056.png)




# 设置Cmder系统右键菜单


1. 将Cmder根目录路径，复制到系统 环境变量 中。  
![image.png](https://qinius.easyhappy.top/avan/202506291716190.png)




![image.png](https://qinius.easyhappy.top/avan/202506291717488.png)




2. 打开 运行 窗口 `Ctrl + R` 输入“cmd”回车  
![image.png](https://qinius.easyhappy.top/avan/202506291717490.png)




然后在新弹出来的窗口中输入 `cmder /register all` 后回车即可  
![image.png](https://qinius.easyhappy.top/avan/202506291717221.png)



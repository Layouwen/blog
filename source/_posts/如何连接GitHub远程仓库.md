---
title: 如何连接GitHub远程仓库
date: 2020-02-12 16:57
tags:
  - 博客
  - github
  - git
  - ssh
categories:
  - 博客
  - ssh
---


# 前置条件


1. GitHub的账号
2. 有类似bash的终端（输指令用）



# 生成ssh key秘钥


官网的教程： [https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)



输入下面的指令



```bash
ssh-keygen -t rsa -b 4096 -C 你的邮箱地址
```



输入完后一直敲回车键，知道出现类似泡泡的东西  
![image.png](https://qinius.easyhappy.top/avan/202506291905909.png)




接着复制生成的路径，使用指令 `cd 路径` 和 `ls` 查看生成的两个文件  
![image.png](https://qinius.easyhappy.top/avan/202506291906683.png)




其中 “id_rsa” 是私钥这个不要给别人看，“id_rsa.pub” 是公钥，我们需要查看这个文件的内容，可以使用 `cat id_rsa.pub` 指令查看该内容，回车后会出现一大串字符  
![image.png](https://qinius.easyhappy.top/avan/202506291906687.png)




将上面的字符一字不漏的复制，打开GitHub的设置页面，找到 “SSH and GPG keys” 然后在又上放单击 “New SSH key”  
![image.png](https://qinius.easyhappy.top/avan/202506291906546.png)




然后在 Title 中给秘钥起个名字，把刚刚复制的内容粘贴到 Key 文本框中，保存关闭即可  
![image.png](https://qinius.easyhappy.top/avan/202506291906603.png)




回到终端输入指令，来接收github发来的公钥，如果显示 “Hi 你的github用户名” 则表示成功了



```bash
ssh -T git@github.com
```



![image.png](https://qinius.easyhappy.top/avan/202506291907834.png)




# 至此就已经连接了你的github账号，如果需要多个多个账号，同样操作一遍即可

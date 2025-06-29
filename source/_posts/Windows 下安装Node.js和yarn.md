---
title: Windows 下安装Node.js和yarn
date: 2020-02-12 16:55
tags:
  - 博客
  - node
  - yarn
categories:
  - 博客
  - windows
---

# 一、Node.js部分


+ 下载Node.js



Node.js官网 [https://nodejs.org/zh-cn/](https://nodejs.org/zh-cn/)

Node.js下载链接 [https://nodejs.org/dist/latest-v10.x/node-v10.19.0-x64.msi](https://nodejs.org/dist/latest-v10.x/node-v10.19.0-x64.msi)



+ ##安装Node.js  
下载后，双击运行文件，一直点 Next 就可以了。如果想改目录可以自行跟改  
![image.png](https://qinius.easyhappy.top/avan/202506291725117.png)

+ 测试是否安装成功



使用 `Ctrl + R` 打卡 运行 窗口，输入“cmd”回车进入 命令行窗口。输入 `node --version` 回车、输入 `npm--version` 回车、输入 `npx--version` 回车，如果都返回版本号则安装成功。没有就重启电脑在重复以上操作。

![image.png](https://qinius.easyhappy.top/avan/202506291725377.png)




+ 配置Node.js



在 命令行窗口 输入 `npm i -g nrm` 回车。下载nrm服务，这里会比较慢，耐心等他下完

![image.png](https://qinius.easyhappy.top/avan/202506291725017.png)




下载完后，输入 `nrm ls` 可以查看所有服务器，使用 `nrm use taobao` 使用淘宝源

![image.png](https://qinius.easyhappy.top/avan/202506291725445.png)




可以输入 `nrm i -g http-server` 顺便下载此服务，来测试一下速度



# 二、yarn部分


+ 下载yarn



yarn官网下载链接 [https://classic.yarnpkg.com/zh-Hans/docs/install#windows-stable](https://classic.yarnpkg.com/zh-Hans/docs/install#windows-stable)



+ 安装yarn



同Node.js一样，无脑下一部即可。安装位置可以自己换



+ 测试是否安装成功



使用 `Ctrl + R` 打卡 运行 窗口，输入“cmd”回车进入 命令行窗口。输入 `yarn --version` 回车，若返回版本号则安装成功。

![image.png](https://qinius.easyhappy.top/avan/202506291726440.png)




+ 配置yarn



在命令行输入 `yarn global add yrm` 回车，等它下载完。

输入 `yrm ls` 回车查看目前服务器，输入 `yrm use taobao` 回车使用淘宝服务器。

![image.png](https://qinius.easyhappy.top/avan/202506291726855.png)




# End


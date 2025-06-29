---
title: VScode中使用Cmder及一些简单命令
date: 2020-02-12 16:43
tags:
  - 博客
  - vscode
  - cmder
categories:
  - 博客
---

# 前置条件


1. 安装了VScode
2. 安装了Cmder  
要是没有安装，可以看前两篇文章  
[前端小白-简单配置VScode及常用快捷键](https://www.jianshu.com/p/ebb9af006e25)  
[前端小白-Windows下安装Cmder及配置&设置系统右键菜单](https://www.jianshu.com/p/050943776aea)



# 配置


打开VScode，使用快捷键 `Ctrl + Shift + P` 输入“settings json”点击“首选项：打开设置（json）”，注意这里是没有“默认”两个字。  
![image.png](https://qinius.easyhappy.top/avan/202506291721368.png)

随便找一行（只要不是最后一行）的逗号后面回车，输入下面代码（[原贴](https://github.com/cmderdev/cmder/wiki/Seamless-VS-Code-Integration#use-cmder-embedded-git-in-vscode)）



```json
"git.enabled": true,
"git.path": "[cmder_root]\\vendor\\git-for-windows\\cmd\\git.exe",
"terminal.integrated.shell.windows": "[cmder_root]\\vendor\\git-for-windows\\bin\\bash.exe",
```



注意里面的[cmder_root]换成你Cmder的根目录路径，并且将单斜杠改为双斜杠。如：



```json
"git.enabled": true,
"git.path": "D:\\Programme\\cmder\\vendor\\git-for-windows\\cmd\\git.exe",
"terminal.integrated.shell.windows": "D:\\Programme\\cmder\\vendor\\git-for-windows\\bin\\bash.exe",
```



![image.png](https://qinius.easyhappy.top/avan/202506291722740.png)




然后 `Ctrl + S` 保存退出，关闭终端重新打开，即可  
![image.png](https://qinius.easyhappy.top/avan/202506291723112.png)




# END

---
title: Mac微信多开以及防撤回方案
date: 2022-04-18 19:16
tags:
  - 博客
  - wechat
categories:
  - 博客
  - wechat
---

## 前言

众所周知，微信无论在 Win 还是 Mac 都是默认不支持多开的。可是我们这些将账号分为公司账号和私人账号的人（海王）。是远不能满足我们的需求。所以只能通过特殊的手段来实现我们的需求。  
在手贱把微信升级之前，你可以直接使用 [WeChatExtension-ForMac](WeChatExtension-ForMac) 安装一下该插件即可实现微信 + 防撤回。  
可在昨天不小心把微信升级到了 3.4.0 后，在次安装发现打开就会弹出签名的错误。（如果你还是可以用，那么恭喜你！下面不用继续看了）。为此我经过艰苦的寻找（百度 2分钟，google 1分钟）才找到了方法。

## 如何防撤回

防撤回可以通过 [WeChatIntercept](WeChatIntercept) 这个插件。我们下载下来后，直接运行里面的 install.sh 脚本自动安装（如果提示 password，那么你输入你的电脑密码即可）。提示安装好后，我们只需要重启微信即可实现防撤回。

## 如何多开

目前我找到了的方案是当你已经打开一个微信后通过命令行，输入以下命令

```bash
open -n /Applications/WeChat.app/Contents/MacOS/WeChat
```

即可另起一个微信进程。

## 不重要的内容

如果你有更好的方案，或者遇到了什么问题。可以在评论或者直接 wx 找我

- 微信：gdgzyw  
- WeChatExtension-ForMac的安装方法：
```bash
sudo rm -r -f WeChatExtension-ForMac && git clone --depth=1 https://github.com/MustangYM/WeChatExtension-ForMac && cd WeChatExtension-ForMac/WeChatExtension/Rely && ./Install.sh && cd ~
```
- [WeChatIntercept防撤回插件](https://codeload.github.com/a244573118/WeChatIntercept/zip/refs/heads/master)
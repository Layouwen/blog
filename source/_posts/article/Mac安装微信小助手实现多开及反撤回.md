---
title: Mac安装微信小助手实现多开及反撤回
date: 2020-07-11 14:39
tags:
  - 博客
  - macos
  - wechat
categories:
  - 博客
  - macos
---

使用终端输入

```bash
sudo rm -r -f WeChatExtension-ForMac && git clone --depth=1 https://github.com/MustangYM/WeChatExtension-ForMac && cd WeChatExtension-ForMac/WeChatExtension/Rely && ./Install.sh
```

等待安装完成即可，完成后重启微信就可以使用了。
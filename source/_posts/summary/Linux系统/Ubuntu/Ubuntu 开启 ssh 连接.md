---
title: Ubuntu 开启 ssh 连接
date: 2023-01-23 15:22:00
tags:
  - 汇总
  - ssh
  - linux
  - ubuntu
categories:
  - 汇总
  - Linux系统
  - Ubuntu
---

如果apt没有更新，先更新

```bash
sudo apt-get update
```

安装ssh服务

```bash
sudo apt-get install openssh-server
```

启动服务

```bash
/etc/init.d/ssh start
```

查看是否启动成功

```bash
netstat -tlp
ps -e | grep ssh
```

> 如果没有安装，先 sudo apt-get install net-tools

开启root登录权限

```bash
vim /etc/ssh/sshd_config
```

PermitRootLogin **yes**

PubkeyAuthentication yes # 支持公钥验证

AuthorizedKeysFile .ssh/authorized_keys # 公钥的文件路径

> 如果没有安装，先 sudo apt-get install vim

将需要远程连接的电脑的pub公钥放到服务器的 authorized_keys 中即可通过 ssh 连接。如果没有设置 ssh 密钥，也可以通过输入密码进入

```bash
ssh [username]@[ipaddress]
```

> 如果失效可能是权限问题  
> chmod 700 ~/.ssh  
> chmod 600 ~/.ssh/authorized_keys
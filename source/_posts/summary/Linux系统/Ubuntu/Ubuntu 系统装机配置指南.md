---
title: Ubuntu 系统装机配置指南
date: 2024-03-23 15:22:00
tags:
  - 汇总
  - linux
  - ubuntu
categories:
  - 汇总
  - Linux系统
  - Ubuntu
---

# 创建用户并提供 sudo 权限

```bash
adduser avan

usermod -aG sudo avan
```

# 安装 docker

官方参考地址: [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)

使用 root 账号安装

```bash
apt-get update
apt-get install ca-certificates curl gnupg
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update

apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

docker run hello-world
```

# 安装 docker-compose

[下载 docker-compose 包](![[Pasted image 20240117133420.png]])

通过 scp 上传到服务器

```bash
scp docker-compose-linux-x86_64 用户名@服务器ip:/home/用户名
```

使用 root 账号进入服务器

```bash
mv /home/用户名/docker-compose-linux-x86_64 /usr/local/bin/docker-compose

cd /usr/local/bin/docker-compose

chmod +x docker-compose
```
---
title: Ubuntu 安装 docker
date: 2023-01-11 15:22:00
tags:
  - 汇总
  - docker
  - linux
  - ubuntu
categories:
  - 汇总
  - Linux系统
  - Ubuntu
---

卸载旧版
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

安装依赖
```bash
sudo apt-get update

sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

安装docker的gpg（阿里）
```bash
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

验证可用性
```bash
sudo apt-key fingerprint 0EBFCD88
```

写入镜像源
```bash
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

再次更新
```bash
sudo apt-get update
```

安装docker
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

开启阿里镜像（最好自己创建一个账号）

[https://cr.console.aliyun.com/cn-qingdao/instances/mirrors](https://cr.console.aliyun.com/cn-qingdao/instances/mirrors)

```bash
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://kolrntba.mirror.aliyuncs.com"]
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
```
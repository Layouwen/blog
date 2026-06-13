---
title: Docker 篇
date: 2024-06-08T20:11:00.000Z
tags:
  - 汇总
  - docker
categories:
  - 汇总
  - 开发环境
uuid: a138b9f2-16e5-4451-9b33-bf63123099d1
---
## windows

下载地址 [https://hub.docker.com/](https://hub.docker.com/)

> 需要登录后才可以下载

下载后，双击安装。接着他会提示你重启。

重启后，下载wsl2内核

下载地址 [https://docs.microsoft.com/zh-cn/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package)

安装结束后，打开 Docker

### 安装desktop报错

```bash
netsh winsock reset
```

## CentOS

卸载旧版本

```shell
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine
```

安装包管理

```shell
sudo yum install -y yum-utils
```

设置镜像

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://kolrntba.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

更新软件包

```shell
yum makecache
```

安装引擎

```shell
yum install docker-ce docker-ce-cli containerd.io
```

启动docker

```shell
systemctl start docker
```

查看版本

```shell
docker version
```

运行 hello world

```shell
docker run hello-world
```

查看镜像

```shell
docker images
```

卸载 docker

```shell
yum remove docker-ce docker-ce-cli containerd.io
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

### docker images 镜像

```shell
docker images
docker images -a # 所有镜像
docker images -q # 只显示id
docker images -aq
```

- REPOSITORY 镜像的仓库源
- TAG 镜像的标签
- IMAGE ID 镜像的id

### docker search 搜索

```shell
docker search mysql

docker search mysql --filter=STARS=3000
```

### docker pull 下载

```shell
docker pull mysql
docker pull mysql:版本号
```

### docker rmi 删除

```shell
docker rmi -f id/名字
```

> 删除全部 `docker rmi -f ${docker images -aq}`

### docker pull 下载容器

```shell
docker pull centos
```

新建容器启动

```shell
docker run [可选参数] image
```

- --name="Name" 容器名字
- -d    后台方式启动
- -it   交互方式进行
- -p    指定容器端口 8080:8080
- -P    随机执行端口

```shell
docker run -it centos /bin/bash
```

查看运行的容器

```shell
docker ps # 查看运行中
docker ps -a # 查看运行过的
```

### 退出容器

```shell
exit
CTRL + P + Q
```

### 删除容器

```shell
docker rm 容器id
docker rm -f ${docker ps -aq}
docker ps -a -q | xargs docker rm
```

### 启动和停止

```shell
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id
```

### 其他命令

后台启动

```shell
docker run -d 镜像名字/id
```

查看日志

```shell
docker logs
docker logs -tf --tail 查看日志的条数 容器
```

- -t 显示日志
- -f 显示时间戳
- --tial 显示指定数量

显示 docker 中的进程信息

```shell
docker top 容器id
```

查看容器内部信息

```shell
docker inspect 容器id
```

进入正在运行的容器

```shell
docker exec -it 容器id /bin/bash # 开启新的终端，可以进行操作
docker attach 容器id # 进入正在执行的终端
```

### 从容器拷贝文件到主机

```shell
docker cp 容器id:容器内路径 目的的主机路径
```

### 可视化管理工具

#### portainer

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/ddocker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

```shell
docker pull redis
```

### commit镜像

```shell
docker commit -m="提交信息" -a="作者"
```

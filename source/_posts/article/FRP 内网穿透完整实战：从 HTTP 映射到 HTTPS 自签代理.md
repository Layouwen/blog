---
title: FRP 内网穿透完整实战：从 HTTP 映射到 HTTPS 自签代理
uuid: d3ff0209-9757-4980-8b56-0ecd04a5c8a8
date: 2026-06-23
tags:
  - 博客
  - frp
categories:
  - 博客
---
![image.png](http://upyun-assets.4van.top/avan/20260623182121895.png)

# FRP 内网穿透完整实战：从 HTTP 映射到 HTTPS 自签代理

在本地开发时，我们经常会遇到这样的需求：

- 本地运行了一个 Vite、Vue、React 或 Node.js 服务；
- 服务只能通过 `127.0.0.1` 访问；
- 希望让外网设备、客户或同事访问；
- 除了普通 HTTP，还希望提供 HTTPS 地址；
- 希望 FRP 服务端和客户端都支持开机自启动。

本文将从零开始搭建一套完整的 FRP 内网穿透环境，并在公网服务器上通过 Nginx 为 FRP 服务增加 HTTPS 入口。

---

## 一、最终实现效果

假设本地 Mac 上运行了三个 HTTP 服务：

```text
http://127.0.0.1:5173
http://127.0.0.1:5174
http://127.0.0.1:5175
```

公网服务器信息：

```text
公网 IP：172.22.22.22
公网域名：custom.com
```

最终提供六个公网访问入口。

### HTTP 访问地址

```text
http://custom.com:30001
http://custom.com:30002
http://custom.com:30003
```

### HTTPS 访问地址

```text
https://custom.com:31001
https://custom.com:31002
https://custom.com:31003
```

端口映射关系如下：

| 本地服务 | HTTP 公网端口 | HTTPS 公网端口 | HTTPS 内部 FRP 端口 |
|---|---:|---:|---:|
| `127.0.0.1:5173` | `30001` | `31001` | `32001` |
| `127.0.0.1:5174` | `30002` | `31002` | `32002` |
| `127.0.0.1:5175` | `30003` | `31003` | `32003` |

---

## 二、整体架构

### HTTP 链路

```text
外网浏览器
    │
    │ http://custom.com:30001
    ▼
公网服务器 frps:30001
    │
    │ FRP 隧道
    ▼
本地 Mac frpc
    │
    ▼
http://127.0.0.1:5173
```

### HTTPS 链路

```text
外网浏览器
    │
    │ https://custom.com:31001
    ▼
公网服务器 Nginx:31001
    │
    │ HTTP 反向代理
    ▼
公网服务器 frps:32001
    │
    │ FRP 隧道
    ▼
本地 Mac frpc
    │
    ▼
http://127.0.0.1:5173
```

完整链路可以理解为：

```text
HTTP  30001 → FRP → 本地 5173
HTTPS 31001 → Nginx → FRP 32001 → 本地 5173

HTTP  30002 → FRP → 本地 5174
HTTPS 31002 → Nginx → FRP 32002 → 本地 5174

HTTP  30003 → FRP → 本地 5175
HTTPS 31003 → Nginx → FRP 32003 → 本地 5175
```

这里需要注意：

- `30001～30003` 由 FRP 直接对外提供 HTTP。
- `31001～31003` 由 Nginx 对外提供 HTTPS。
- `32001～32003` 是 Nginx 转发到 FRP 的内部上游端口。
- HTTPS 由公网服务器上的 Nginx 负责终止。
- 本地服务仍然只需要运行普通 HTTP。

---

## 三、准备条件

需要准备：

1. 一台拥有公网 IP 的 Linux 服务器。
2. 一个解析到公网服务器的域名。
3. 本地 Mac 上能够正常访问的 HTTP 服务。
4. 云服务器安全组允许相关端口。
5. FRP 服务端和客户端尽量使用相同版本。

本文示例：

```text
FRP 版本：0.69.1
公网 IP：172.22.22.22
公网域名：custom.com
FRP 控制端口：7000
```

先给域名添加 DNS 解析：

```text
记录类型：A
主机记录：@
记录值：172.22.22.22
```

检查解析：

```bash
dig +short custom.com
```

预期返回：

```text
172.22.22.22
```

---

# 服务端安装 frps

## 四、确认服务器架构

登录公网服务器：

```bash
ssh root@172.22.22.22
```

查看服务器架构：

```bash
uname -m
```

对应关系：

| 系统输出 | FRP 安装包 |
|---|---|
| `x86_64` | `linux_amd64` |
| `aarch64` | `linux_arm64` |
| `arm64` | `linux_arm64` |

下面以 `linux_amd64` 为例。

---

## 五、下载 FRP

设置版本变量：

```bash
FRP_VERSION=0.69.1
```

使用 `wget` 下载：

```bash
wget "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz"
```

没有 `wget` 时，可以使用：

```bash
curl -LO "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz"
```

解压：

```bash
tar -xzvf "frp_${FRP_VERSION}_linux_amd64.tar.gz"
```

进入目录：

```bash
cd "frp_${FRP_VERSION}_linux_amd64"
```

检查版本：

```bash
./frps -v
```

---

## 六、安装 frps

创建配置目录：

```bash
sudo mkdir -p /etc/frp
```

复制程序：

```bash
sudo cp ./frps /usr/local/bin/frps
sudo chmod +x /usr/local/bin/frps
```

确认安装结果：

```bash
/usr/local/bin/frps -v
```

---

## 七、生成认证 Token

执行：

```bash
openssl rand -hex 32
```

输出示例：

```text
8e1d9cc7f7c8b0562f4f984b0fb930a66a5b2d9f48045a554d8f8ef99e9c503d
```

保存这个 Token，服务端和客户端必须使用相同值。

不要把真实 Token 提交到公开仓库或发布到文章中。

---

## 八、创建 frps 配置

创建配置文件：

```bash
sudo vim /etc/frp/frps.toml
```

写入：

```toml
# frpc 连接 frps 的控制端口
bindPort = 7000

# Token 认证
auth.method = "token"
auth.token = "替换成你的随机Token"

# 日志输出到 systemd journal
log.to = "console"
log.level = "info"
```

这里不配置：

```toml
proxyBindAddr = "127.0.0.1"
```

因为本文中的 `30001～30003` 需要由 FRP 直接对公网提供 HTTP。

`32001～32003` 虽然也由 FRP 监听，但不应该在云安全组中对公网开放。

---

## 九、校验 frps 配置

```bash
sudo /usr/local/bin/frps verify -c /etc/frp/frps.toml
```

正常输出类似：

```text
frps: the configuration file /etc/frp/frps.toml syntax is ok
```

---

## 十、创建 frps systemd 服务

创建服务文件：

```bash
sudo vim /etc/systemd/system/frps.service
```

写入：

```ini
[Unit]
Description=FRP Server
Documentation=https://github.com/fatedier/frp
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/local/bin/frps -c /etc/frp/frps.toml

Restart=always
RestartSec=5

LimitNOFILE=1048576
NoNewPrivileges=true
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重新加载 systemd：

```bash
sudo systemctl daemon-reload
```

启动服务并设置开机自启：

```bash
sudo systemctl enable --now frps
```

查看状态：

```bash
sudo systemctl status frps
```

正常应看到：

```text
Active: active (running)
```

确认开机自启：

```bash
systemctl is-enabled frps
```

预期输出：

```text
enabled
```

确认运行状态：

```bash
systemctl is-active frps
```

预期输出：

```text
active
```

---

## 十一、frps 常用管理命令

启动：

```bash
sudo systemctl start frps
```

停止：

```bash
sudo systemctl stop frps
```

重启：

```bash
sudo systemctl restart frps
```

查看状态：

```bash
sudo systemctl status frps
```

查看实时日志：

```bash
sudo journalctl -u frps -f
```

查看最近 100 行日志：

```bash
sudo journalctl -u frps -n 100 --no-pager
```

修改 `frps.toml` 后：

```bash
sudo /usr/local/bin/frps verify -c /etc/frp/frps.toml && \
sudo systemctl restart frps && \
sudo systemctl status frps
```

如果修改的是 `frps.service`：

```bash
sudo systemctl daemon-reload && \
sudo systemctl restart frps && \
sudo systemctl status frps
```

查找所有以 `frp` 开头的 systemd 服务：

```bash
systemctl list-unit-files --type=service | grep -i '^frp'
```

---

# 客户端安装 frpc

## 十二、下载 Mac 客户端

Apple Silicon Mac 使用 `darwin_arm64`：

```bash
cd ~/Downloads

FRP_VERSION=0.69.1

curl -LO "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_darwin_arm64.tar.gz"

tar -xzvf "frp_${FRP_VERSION}_darwin_arm64.tar.gz"

cd "frp_${FRP_VERSION}_darwin_arm64"
```

检查版本：

```bash
./frpc -v
```

Intel Mac 应使用：

```text
darwin_amd64
```

---

## 十三、配置单个 HTTP 服务

如果只需要把本地 `5173` 映射到公网 `30001`，可以使用最小配置。

创建：

```bash
vim frpc.toml
```

写入：

```toml
serverAddr = "custom.com"
serverPort = 7000

auth.method = "token"
auth.token = "替换成和服务端相同的随机Token"

log.to = "console"
log.level = "info"

[[proxies]]
name = "local-5173-http"
type = "tcp"

localIP = "127.0.0.1"
localPort = 5173

remotePort = 30001

transport.useEncryption = true
```

映射关系：

```text
http://custom.com:30001
        ↓
http://127.0.0.1:5173
```

校验：

```bash
./frpc verify -c ./frpc.toml
```

启动：

```bash
./frpc -c ./frpc.toml
```

---

## 十四、同时配置 HTTP 和 HTTPS 上游

如果同一个本地服务既要提供 HTTP，又要提供 HTTPS，需要为它配置两个 FRP 代理。

完整 `frpc.toml`：

```toml
serverAddr = "custom.com"
serverPort = 7000

auth.method = "token"
auth.token = "替换成和服务端相同的随机Token"

log.to = "console"
log.level = "info"

# ==================================================
# 本地服务 5173
# ==================================================

# HTTP：http://custom.com:30001
[[proxies]]
name = "service-5173-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5173
remotePort = 30001
transport.useEncryption = true

# HTTPS 上游：Nginx 31001 → FRP 32001
[[proxies]]
name = "service-5173-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5173
remotePort = 32001
transport.useEncryption = true

# ==================================================
# 本地服务 5174
# ==================================================

# HTTP：http://custom.com:30002
[[proxies]]
name = "service-5174-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5174
remotePort = 30002
transport.useEncryption = true

# HTTPS 上游：Nginx 31002 → FRP 32002
[[proxies]]
name = "service-5174-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5174
remotePort = 32002
transport.useEncryption = true

# ==================================================
# 本地服务 5175
# ==================================================

# HTTP：http://custom.com:30003
[[proxies]]
name = "service-5175-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5175
remotePort = 30003
transport.useEncryption = true

# HTTPS 上游：Nginx 31003 → FRP 32003
[[proxies]]
name = "service-5175-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5175
remotePort = 32003
transport.useEncryption = true
```

校验配置：

```bash
./frpc verify -c ./frpc.toml
```

启动：

```bash
./frpc -c ./frpc.toml
```

---

# 配置 HTTPS 自签证书

## 十五、为什么还需要 Nginx

FRP TCP 代理只负责转发 TCP 流量，它不会自动为本地 HTTP 服务增加 HTTPS。

因此需要在公网服务器增加 Nginx：

```text
HTTPS 请求
    ↓
Nginx 负责 TLS 解密
    ↓
转发到 FRP 的 HTTP 上游端口
    ↓
本地 HTTP 服务
```

本地服务仍然可以保持：

```text
http://127.0.0.1:5173
```

不需要给 Vite、Vue 或 Node.js 单独配置证书。

---

## 十六、生成自签证书

在准备存放证书的目录中执行：

```bash
DOMAIN="custom.com"

openssl req \
  -x509 \
  -nodes \
  -newkey rsa:2048 \
  -sha256 \
  -days 3650 \
  -keyout "./$DOMAIN.key" \
  -out "./$DOMAIN.crt" \
  -subj "/CN=$DOMAIN" \
  -addext "subjectAltName=DNS:$DOMAIN" && \
chmod 600 "./$DOMAIN.key"
```

生成结果：

```text
./custom.com.crt
./custom.com.key
```

检查证书：

```bash
openssl x509 \
  -in ./custom.com.crt \
  -noout \
  -subject \
  -issuer \
  -dates \
  -ext subjectAltName
```

应当包含：

```text
DNS:custom.com
```

需要注意：

> 自签证书即使域名完全匹配，浏览器仍然会提示证书不受信任。  
> 这是因为证书不是由系统信任的 CA 机构签发。

测试时可以使用：

```bash
curl -k
```

正式使用建议改为 Let's Encrypt 或其他受信任证书。

---

# Docker 部署 Nginx

## 十七、为什么推荐使用 host 网络

如果 Nginx 运行在 Docker 容器中，而 frps 运行在宿主机 systemd 中，那么容器内的：

```text
127.0.0.1
```

默认表示 Nginx 容器自身，不是宿主机。

使用 Docker host 网络后：

```yaml
network_mode: host
```

Nginx 容器与宿主机共用网络栈。

这样容器内可以直接访问：

```text
127.0.0.1:32001
127.0.0.1:32002
127.0.0.1:32003
```

同时也不需要逐个配置 Docker `ports` 映射。

---

## 十八、Nginx Docker Compose 配置

目录结构示例：

```text
nginx/
├── docker-compose.yml
└── conf.d/
    ├── frp.conf
    └── cert/
        ├── custom.com.crt
        └── custom.com.key
```

`docker-compose.yml`：

```yaml
services:
  nginx:
    image: nginx:1.29
    container_name: nginx
    restart: always

    network_mode: host

    volumes:
      - ./conf.d:/etc/nginx/conf.d:ro
```

使用 host 网络后，不要再配置：

```yaml
ports:
  - "31001:31001"
```

因为 host 网络模式下，Nginx 会直接监听宿主机端口。

启动：

```bash
docker compose up -d
```

修改 Compose 配置后重新创建：

```bash
docker compose down
docker compose up -d
```

查看日志：

```bash
docker logs nginx -n 200 -f
```

---

## 十九、创建 Nginx HTTPS 配置

创建：

```bash
vim conf.d/frp.conf
```

写入：

```nginx
map $http_upgrade $connection_upgrade {
    default upgrade;
    ''      close;
}

# ==================================================
# HTTPS 31001 → FRP 32001 → 本地 5173
# ==================================================

server {
    listen 31001 ssl;
    listen [::]:31001 ssl;

    server_name custom.com;

    ssl_certificate     /etc/nginx/conf.d/cert/custom.com.crt;
    ssl_certificate_key /etc/nginx/conf.d/cert/custom.com.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    location / {
        proxy_pass http://127.0.0.1:32001;

        proxy_http_version 1.1;

        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-Port 31001;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        proxy_buffering off;
        proxy_cache off;

        proxy_connect_timeout 10s;
        proxy_send_timeout 3600s;
        proxy_read_timeout 3600s;
    }
}

# ==================================================
# HTTPS 31002 → FRP 32002 → 本地 5174
# ==================================================

server {
    listen 31002 ssl;
    listen [::]:31002 ssl;

    server_name custom.com;

    ssl_certificate     /etc/nginx/conf.d/cert/custom.com.crt;
    ssl_certificate_key /etc/nginx/conf.d/cert/custom.com.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    location / {
        proxy_pass http://127.0.0.1:32002;

        proxy_http_version 1.1;

        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-Port 31002;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        proxy_buffering off;
        proxy_cache off;

        proxy_connect_timeout 10s;
        proxy_send_timeout 3600s;
        proxy_read_timeout 3600s;
    }
}

# ==================================================
# HTTPS 31003 → FRP 32003 → 本地 5175
# ==================================================

server {
    listen 31003 ssl;
    listen [::]:31003 ssl;

    server_name custom.com;

    ssl_certificate     /etc/nginx/conf.d/cert/custom.com.crt;
    ssl_certificate_key /etc/nginx/conf.d/cert/custom.com.key;

    ssl_protocols TLSv1.2 TLSv1.3;

    location / {
        proxy_pass http://127.0.0.1:32003;

        proxy_http_version 1.1;

        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-Port 31003;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;

        proxy_buffering off;
        proxy_cache off;

        proxy_connect_timeout 10s;
        proxy_send_timeout 3600s;
        proxy_read_timeout 3600s;
    }
}
```

说明：

```nginx
listen 31001 ssl;
```

表示监听 IPv4 的 `31001` HTTPS 端口。

```nginx
listen [::]:31001 ssl;
```

表示监听 IPv6 的 `31001` HTTPS 端口。

只需要 IPv4 时，可以只保留：

```nginx
listen 31001 ssl;
```

---

## 二十、检查 Nginx 配置

查看 Nginx 是否加载了 `frp.conf`：

```bash
docker exec -it nginx nginx -T
```

只查找相关配置：

```bash
docker exec -it nginx nginx -T 2>&1 | grep -A 20 -B 2 "frp.conf"
```

测试语法：

```bash
docker exec -it nginx nginx -t
```

正常输出：

```text
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

重新加载：

```bash
docker exec -it nginx nginx -s reload
```

或者直接重启：

```bash
docker restart nginx
```

---

# 开放服务器端口

## 二十一、云安全组配置

需要开放以下 TCP 端口：

| 端口 | 是否公网开放 | 用途 |
|---:|---|---|
| `7000` | 是 | frpc 连接 frps |
| `30001～30003` | 是 | HTTP 公网入口 |
| `31001～31003` | 是 | HTTPS 公网入口 |
| `32001～32003` | 否 | Nginx 内部转发到 FRP |
| `5173～5175` | 否 | 本地 Mac 服务 |

推荐规则：

| 协议 | 端口范围 | 来源 |
|---|---:|---|
| TCP | `7000` | 建议限制为本地出口 IP |
| TCP | `30001-30003` | 根据实际访问范围 |
| TCP | `31001-31003` | 根据实际访问范围 |

不要在云安全组中开放：

```text
32001-32003
```

因为这些端口只供公网服务器上的 Nginx 使用。

---

## 二十二、服务器防火墙配置

使用 UFW：

```bash
sudo ufw allow 7000/tcp
sudo ufw allow 30001:30003/tcp
sudo ufw allow 31001:31003/tcp
sudo ufw reload
```

查看规则：

```bash
sudo ufw status numbered
```

使用 firewalld：

```bash
sudo firewall-cmd --permanent --add-port=7000/tcp
sudo firewall-cmd --permanent --add-port=30001-30003/tcp
sudo firewall-cmd --permanent --add-port=31001-31003/tcp
sudo firewall-cmd --reload
```

---

# 验证链路

## 二十三、检查服务端监听端口

执行：

```bash
sudo ss -lntp | grep -E '7000|3000[1-3]|3100[1-3]|3200[1-3]'
```

预期结果：

```text
frps 监听：7000
frps 监听：30001～30003
frps 监听：32001～32003
nginx 监听：31001～31003
```

---

## 二十四、测试 FRP HTTP 入口

```bash
curl -v http://custom.com:30001
curl -v http://custom.com:30002
curl -v http://custom.com:30003
```

---

## 二十五、测试 HTTPS 内部上游

在公网服务器执行：

```bash
curl -v http://127.0.0.1:32001
curl -v http://127.0.0.1:32002
curl -v http://127.0.0.1:32003
```

如果这里访问失败，说明问题位于：

```text
frps → frpc → 本地服务
```

常见原因：

- 本地服务没有启动；
- `frpc` 没有启动；
- `remotePort` 配置错误；
- `localPort` 配置错误；
- Token 不一致；
- FRP 客户端连接失败。

---

## 二十六、测试 HTTPS 入口

因为是自签证书，需要添加 `-k`：

```bash
curl -vk https://custom.com:31001
curl -vk https://custom.com:31002
curl -vk https://custom.com:31003
```

如果返回：

```text
502 Bad Gateway
```

说明：

```text
浏览器 → Nginx
```

已经成功，但：

```text
Nginx → FRP 32001
```

访问失败。

如果返回：

```text
Connection refused
```

重点检查：

- Nginx 是否监听 `31001`；
- 云安全组是否开放 `31001`；
- Linux 防火墙是否开放 `31001`；
- Nginx Docker 是否使用 host 网络；
- Nginx 配置是否成功加载。

---

# 最终配置汇总

## 二十七、服务端 frps.toml

```toml
bindPort = 7000

auth.method = "token"
auth.token = "替换成你的随机Token"

log.to = "console"
log.level = "info"
```

---

## 二十八、客户端 frpc.toml

```toml
serverAddr = "custom.com"
serverPort = 7000

auth.method = "token"
auth.token = "替换成和服务端相同的随机Token"

log.to = "console"
log.level = "info"

# 本地 5173：HTTP + HTTPS
[[proxies]]
name = "service-5173-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5173
remotePort = 30001
transport.useEncryption = true

[[proxies]]
name = "service-5173-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5173
remotePort = 32001
transport.useEncryption = true

# 本地 5174：HTTP + HTTPS
[[proxies]]
name = "service-5174-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5174
remotePort = 30002
transport.useEncryption = true

[[proxies]]
name = "service-5174-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5174
remotePort = 32002
transport.useEncryption = true

# 本地 5175：HTTP + HTTPS
[[proxies]]
name = "service-5175-http"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5175
remotePort = 30003
transport.useEncryption = true

[[proxies]]
name = "service-5175-https-upstream"
type = "tcp"
localIP = "127.0.0.1"
localPort = 5175
remotePort = 32003
transport.useEncryption = true
```

---

## 二十九、最终访问地址

HTTP：

```text
http://custom.com:30001
http://custom.com:30002
http://custom.com:30003
```

HTTPS：

```text
https://custom.com:31001
https://custom.com:31002
https://custom.com:31003
```

最终映射关系：

```text
HTTP  30001 → FRP → 本地 5173
HTTPS 31001 → Nginx → FRP 32001 → 本地 5173

HTTP  30002 → FRP → 本地 5174
HTTPS 31002 → Nginx → FRP 32002 → 本地 5174

HTTP  30003 → FRP → 本地 5175
HTTPS 31003 → Nginx → FRP 32003 → 本地 5175
```

---

# 常见问题

## 1. 为什么 HTTPS 不能直接使用 FRP 的 30001 端口

同一个 TCP 端口不能同时由 frps 和 Nginx 监听。

如果 `30001` 已经被 frps 占用，Nginx 就不能再监听：

```nginx
listen 30001 ssl;
```

所以本文采用：

```text
HTTP：30001
HTTPS：31001
HTTPS 内部 FRP：32001
```

---

## 2. 为什么 HTTPS 需要额外配置一个 32001

因为 `31001` 已经由 Nginx 占用。

Nginx 接收到 HTTPS 请求后，需要有另一个 FRP 端口把请求转发到本地，因此使用：

```text
32001
```

完整链路：

```text
31001 → Nginx → 32001 → FRP → 5173
```

---

## 3. 为什么 Docker 中不能直接访问宿主机 127.0.0.1

普通 Docker 网络模式下：

```text
容器中的 127.0.0.1 = 容器自身
```

不代表宿主机。

使用：

```yaml
network_mode: host
```

后，容器与宿主机共用网络栈，因此可以直接访问：

```text
127.0.0.1:32001
```

---

## 4. 为什么 Nginx 日志没有显示加载 frp.conf

Nginx 启动日志默认不会逐个打印所有加载的配置文件。

应使用：

```bash
docker exec -it nginx nginx -T
```

查看最终生效配置。

---

## 5. 为什么自签证书浏览器仍然提示风险

因为自签证书不是由浏览器信任的 CA 机构签发。

即使证书中的域名完全正确：

```text
DNS:custom.com
```

浏览器仍然会提示：

```text
证书颁发机构不受信任
```

可以：

- 在客户端系统中导入并信任该证书；
- 或者使用 Let's Encrypt 等受信任证书。

---

## 6. 修改配置后如何更新服务

修改 frps：

```bash
sudo /usr/local/bin/frps verify -c /etc/frp/frps.toml && \
sudo systemctl restart frps
```

修改 Nginx 配置：

```bash
docker exec -it nginx nginx -t && \
docker exec -it nginx nginx -s reload
```

修改 Docker Compose：

```bash
docker compose down
docker compose up -d
```

---

## 总结

这套方案把 FRP 与 Nginx 的职责进行了清晰拆分：

- FRP 负责公网服务器和本地 Mac 之间的内网穿透；
- Nginx 负责公网 HTTPS、证书和反向代理；
- 本地应用只需要运行普通 HTTP；
- HTTP 和 HTTPS 可以同时保留；
- Docker host 网络可以减少端口映射和宿主机访问配置。

核心端口规划：

```text
7000         FRP 控制连接
30001-30003  公网 HTTP
31001-31003  公网 HTTPS
32001-32003  HTTPS 对应的 FRP 内部上游
5173-5175    本地应用端口
```

最终可以通过下面的地址访问本地服务：

```text
http://custom.com:30001
https://custom.com:31001
```

并按照同样的规则继续扩展更多端口和本地服务。
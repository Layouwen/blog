---
title: SSL 证书自签
date: 2026-02-06T17:18:54.000Z
tags:
  - 汇总
  - SSL
categories:
  - 汇总
uuid: 724486f0-d531-40d5-b6bb-5b40e01d7c29
---

以签发 `upyun-assets.4van.top` 为例, 后续替换为对应的域名

```bash
openssl req -x509 -newkey rsa:2048 -nodes -keyout server.key -out server.crt -days 3650 -subj "/C=CN/ST=Beijing/L=Beijing/O=MyOrg/OU=IT/CN=upyun-assets.4van.top"
```

生成根证书 (Root CA) 创建虚拟的“CA机构”

```bash
openssl genrsa -out rootCA.key 2048
```

利用私钥生成自签名的根证书, 这里的 Common Name 的问题可以随便填

```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.crt
```

```c
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:GuangDong
Locality Name (eg, city) []:GuangZhou
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:Avan Root CA
Email Address []:layouwen@gmail.com
```

生成网站证书 (Server Cert)：为 upyun-assets.4van.top 创建一个证书签发申请，并用根证书签名。

```powershell
type nul > san.cnf
```

```c
[req]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[dn]
C=CN
ST=Beijing
L=Beijing
O=MyOrg
OU=IT
CN=upyun-assets.4van.top

[req_ext]
subjectAltName = @alt_names

[alt_names]
DNS.1 = upyun-assets.4van.top
```

创建 csr

```bash
openssl req -new -key server.key -out server.csr -config san.cnf
```

使用根证书签发网站证书，有效期1年

```bash
openssl x509 -req -in server.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out server.crt -days 365 -sha256 -extensions req_ext -extfile san.cnf
```

 合并完整
 
```powershell
type server.crt rootCA.crt > fullchain.crt
```

(可选) 可用此命令验证证书链是否完整
```bash
openssl verify -CAfile rootCA.crt fullchain.crt
```

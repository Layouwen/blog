---
title: docker 经常用的命令
date: 2022-01-23 15:22:00
tags:
  - 汇总
  - docker
categories:
  - 汇总
  - docker
---

# Windows 启动容器

```bash
MSYS_NO_PATHCONV=1 docker run -it -p 1883:1883 -v $(pwd)/mosquitto.conf:/mosquitto/config/mosquitto.conf eclipse-mosquitto
```

> MSYS_NO_PATHCONV=1 和 $(pwd) 是在 windows 中 gitbash 运行命令需要添加的参数，否则会识别不了路径

# Windows docker 启动 mysql 服务

```bash
MSYS_NO_PATHCONV=1 docker run --name MYSQL服务名字 -it -p 映射出来端口:3306 -v $(pwd)/mysql/conf/my.conf:/etc/mysql/my.conf -v ${pwd}/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=数据库密码 mysql:5.7
```

# 启动 nginx 服务

```bash
docker run -d --name nginx nginx

docker cp nginx:/usr/share/nginx/html /Users/avan/services/nginx/html
docker cp nginx:/etc/nginx/conf.d /Users/avan/services/nginx/conf.d
docker cp nginx:/etc/nginx/nginx.conf /Users/avan/services/nginx/nginx.conf
```

```bash
docker run --name main-nginx --rm -p 80:80 -p 443:443 -v /Users/avan/services/nginx/html:/usr/share/nginx/html -v /Users/avan/services/nginx/conf.d:/etc/nginx/conf.d nginx
```

## 转发 ws

```nginx
    location ^~ /videows {
	    proxy_pass http://192.168.10.11:9999;
 proxy_read_timeout 300s;
                proxy_send_timeout 300s;
                proxy_set_header  Host $http_host;
                proxy_set_header  X-Real-IP  $remote_addr;
                proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header  X-Forwarded-Proto $scheme;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection $connection_upgrade;
    }
```

```nginx
server {
        listen 80;

        gzip on;
        gzip_min_length 1k;
        gzip_comp_level 9;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        gzip_vary on;
        gzip_disable "MSIE [1-6]\.";
        absolute_redirect off;
        client_max_body_size 50m;

        location / {
                add_header Access-Control-Allow-Origin *;
                return 301 /web/unify;
        }

        location ^~ /web/unify {
                add_header Access-Control-Allow-Origin *;
                alias /usr/share/nginx/html/unify;
                try_files $uri $uri/unify /web/unify/index.html;
        }

        location ^~ /web/station {
                add_header Access-Control-Allow-Origin *;
                alias /usr/share/nginx/html/station;
                try_files $uri $uri/station /web/station/index.html;
        }
        location ^~ /web/mobile {
                add_header Access-Control-Allow-Origin *;
                proxy_pass http://staging-flow-app:8080;
        }

        location ^~ /web/auth {
                add_header Access-Control-Allow-Origin *;
                alias /usr/share/nginx/html/auth;
                try_files $uri $uri/auth /web/auth/index.html;
        }

        location ^~ /station/ {
                add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Methods *;
            add_header Access-Control-Allow-Headers *;
            rewrite ^/station/(.*)$ /$1 break;
            proxy_pass http://staging-flow-station:8000;
        }

        location ^~ /flowtrace/socketio/ {
            add_header backendIP $upstream_addr;
            add_header backendCode $upstream_status;
           proxy_redirect off;
            proxy_connect_timeout 6000;
            proxy_read_timeout 6000;
            proxy_send_timeout 6000;
            proxy_set_header Host staging-flow-flow:8008;
           proxy_pass http://staging-flow-flow:8008/socket.io/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
-- VISUAL LINE --                                                                                                                                     1,1           Top
        }


        location ^~ /auth/ {
                add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Methods *;
            add_header Access-Control-Allow-Headers *;
            rewrite ^/auth/(.*)$ /$1 break;
            proxy_pass http://staging-flow-auth:8001;
        }

        location ^~ /asset/socketio/ {
            add_header backendIP $upstream_addr;
            add_header backendCode $upstream_status;
            proxy_redirect off;
            proxy_connect_timeout 6000;
            proxy_read_timeout 6000;
            proxy_send_timeout 6000;
            proxy_set_header Host staging-unify-asset:8009;
            proxy_pass http://staging-unify-asset:8009/socket.io/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

        location ^~ /asset/ {
            rewrite ^/asset/(.*)$ /$1 break;
            proxy_pass http://staging-unify-asset:8009;
        }

        location ^~ /emergency/ {
            rewrite ^/emergency/(.*)$ /$1 break;
            proxy_pass http://staging-unify-emergency:8007;
        }

        location ^~ /station/socketio/ {
            add_header backendIP $upstream_addr;
            add_header backendCode $upstream_status;
            proxy_redirect off;
            proxy_connect_timeout 6000;
            proxy_read_timeout 6000;
            proxy_send_timeout 6000;
            proxy_set_header Host staging-flow-station:8000;
            proxy_pass http://staging-flow-station:8000/socket.io/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

        #�~E代�~P~F�~K�~U使�~T�
        # location ^~ /agent {
            #     add_header Access-Control-Allow-Origin *;
        #     add_header Access-Control-Allow-Methods *;
        #     add_header Access-Control-Allow-Headers *;
        #     rewrite ^/agent(.*)$ /$1 break;
        #     proxy_pass http://staging-flow-agent:6000;
        # }


    }
```

# 常用命令

## 创建 mysql 服务

```bash
docker run -itd --name mysql-server -v /Users/epath/data:/var/lib/mysql -v /etc/timezone:/etc/timezone -v /etc/localtime:/etc/localtime -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123123 mysql
```

## 创建 mongo 服务

```bash
docker run \
--name some-mongo \
-v 本地db映射路径:/data/db \
-d mongo
```

## 创建 redis 服务

```bash
docker run --name redis-server -p 6379:6379 -d redis
```
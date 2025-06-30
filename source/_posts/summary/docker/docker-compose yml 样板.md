---
title: docker-compose yml 样板
tags:
  - 汇总
  - docker-compose
categories:
  - 汇总
  - docker
---

# 启动 mongo

```yml
services:

  mongo:
    image: mongo:7.0.5
    restart: always
    ports:
      - 27000:27017
    volumes:
      - /home/avan/docker-compose/base-env/mongo/data/db:/data/db
      - /home/avan/docker-compose/base-env/mongo/backup:/backup
    environment:
      MONGO_INITDB_ROOT_USERNAME: 用户名
      MONGO_INITDB_ROOT_PASSWORD: 密码
```

# 启动 postgres

```yml
services:

  postgres:
    image: postgres:16
    restart: always
    ports:
      - 27100:5432
    volumes:
      - /home/avan/docker-compose/base-env/postgres/data:/var/lib/postgresql/data
      - /home/avan/docker-compose/base-env/postgres/backup:/backup
    environment:
      POSTGRES_PASSWORD: 密码
```

# 启动 nginx

TODO
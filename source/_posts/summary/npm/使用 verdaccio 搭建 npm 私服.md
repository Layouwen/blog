---
title: 使用 verdaccio 搭建 npm 私服
date: 2024-06-30 17:18:54
tags:
  - 汇总
  - npm
categories:
  - 汇总
  - npm
---

# 启动 docker 服务

```bash
V_PATH=<数据文件目录路径>; docker run -it -u 1000 --name verdaccio \
-p 4873:4873 \
-v $V_PATH/conf:/verdaccio/conf \
-v $V_PATH/storage:/verdaccio/storage \
-v $V_PATH/plugins:/verdaccio/plugins \
-e VERDACCIO_PUBLIC_URL='协议://域名' \
verdaccio/verdaccio
```

> -u 是对应用户的 uid

**conf/config.yaml**

[官方参考文件](https://github.com/verdaccio/verdaccio/tree/master/packages/config/src/conf)

```yaml
# 包存储的路径
storage: /verdaccio/storage

auth:
  htpasswd:
    file: /verdaccio/conf/htpasswd
    # -1 不允许注册
    # 不设置则不限制
    max_users: -1
security:
  api:
    jwt:
      sign:
        expiresIn: 60d
        notBefore: 1
  web:
    sign:
      expiresIn: 7d

# 上传的包大小
max_body_size: 300mb

# 链接其他 npm 源
uplinks:
  npmjs:
    url: https://registry.npmmirror.com/

packages:
  '@whalewave/*':
    access: $authenticated
    publish: $authenticated

  '@*/*':
    # 范围限制的包
    access: $all
    publish: $authenticated
    proxy: npmjs

  '**':
    access: $all
    publish: $authenticated
    proxy: npmjs

# To use `npm audit` uncomment the following section
middlewares:
  audit:
    enabled: true

# log settings
log:
  - { type: stdout, format: pretty, level: trace }
  #- {type: file, path: verdaccio.log, level: info}
```

**conf/htpasswd**

```
admin:$2y$10$f3jh1gL0lcx2J6PwK0/4VOMBIQgOHxV3ZU/.vPSX1unZ4DL9F07da
lzm:$2y$10$f3jh1gL0lcx2J6PwK0/4VOMBIQgOHxV3ZU/.vPSX1unZ4DL9F07da
```

> 账号: admin   
> 密码: admin

# htpasswd 注册账号

[https://hostingcanada.org/htpasswd-generator/](https://hostingcanada.org/htpasswd-generator/)

# 下载

```bash
npm config set registry <协议://域名>
npm login --registry <协议://域名>
npm config set always-auth true
npm i @whalewave/adm-ui
```

```tsx
import { Button } from '@whalewave/adm-ui'

export default () => {
  return <div>
    <Button type="lg"/>
    <Button type="sm"/>
  </div>
}
```

```bash
# 查看配置文件
npm config get registry
# 还原
npm config set registry https://registry.npmmirror.com/
```

# 重启

```bash
VERDACCIO_PUBLIC_URL='<协议://域名>'; docker restart 容器id
```
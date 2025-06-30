---
title: nods中mysql时间相差8小时
date: 2022-01-16 21:17
tags:
  - 博客
  - 后端
  - mysql
  - nestjs
categories:
  - 博客
  - 后端
  - nodejs
---

## 前言

最近在做自己的一个记账项目，后端nestjs中使用typeorm的mysql。当添加记录时，发现所以时间都相差了8小时。
后面查了一下资料发现因为默认 timezone 是用 UTC 的。所以只需要设置成我们自己的时区即可。

## 解决方法


**ormconfig.json**
```json
{
  "type": "mysql",
  "host": "localhost",
  "port": 3306,
  "username": "",
  "password": "",
  "database": "development",
  "entities": [
    "dist/**/*.entity{.ts,.js}"
  ],
  "synchronize": true,
  "timezone": "+08:00" // 添加这一条
}
```

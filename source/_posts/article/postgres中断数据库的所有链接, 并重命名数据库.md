---
title: 'postgres 中断数据库的所有链接, 并重命名数据库'
date: 2023-05-16T17:18:54.000Z
tags:
  - 博客
  - postgres
  - 后端
categories:
  - 博客
  - 后端
uuid: 3e7997bf-a90a-4081-a3ab-a6ed5b22582e
---

```bash
# docker 直接管理员进入
psql -U postgres
# 查看连接数
SELECT COUNT(*) AS connection_count
FROM pg_stat_activity
WHERE datname = '数据库名字';
# 删除所有连接
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE datname = '数据库名字'
  AND pid <> pg_backend_pid();
# 重命名
ALTER DATABASE "原本名字" rename TO "新名字";
```

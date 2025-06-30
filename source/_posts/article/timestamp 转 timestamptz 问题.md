---
title: timestamp 转 timestamptz 问题
date: 2024-09-12 21:13:54
tags:
  - 后端
  - postgres
categories:
  - 博客
  - 后端
---


无时区转带时区的格式的时候, 使用 sync 他会将你原本的字段删除, 重新创建一个类型为 timestamptz 的字段, 数据会完全清空.

建议新增一个 swaptime 字段, 将原本的 time 先转到 swaptime, 确认没问题后. 对服务器进行暂停维护, 然后开始将 time 的字段去掉, 然后将 swaptime 的名字改成 time. 重新开启服务.
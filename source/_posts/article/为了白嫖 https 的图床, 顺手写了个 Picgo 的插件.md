---
title: 为了白嫖 https 的图床, 顺手写了个 Picgo 的插件
date: 2025-07-01 17:18:54
tags:
  - 博客
  - picgo
  - 图床
categories:
  - 博客
  - 图床
---

![image.png](https://s2.loli.net/2025/07/01/xkyRgZGKVIqXev8.png)

## **🧠 前言**

  

> 本文没有什么深奥的技术点，纯粹是个人使用 Obsidian + Hexo 搭博客 + 图片服务的折腾记录，顺便+1篇博客写作练习。

---

## **背景**

  

一直以来，我用 **Obsidian** 做“第二大脑”做知识沉淀。某天，「张三」想看我的一些笔记文章，于是我顺手用 **Hexo** 搭了个 [博客](https://blog.4van.top)，还加了 **HTTPS**。之前文章中插图都是用免费的 **七牛云 HTTP 图床 + PicGo 上传**，但是 HTTPS 的站点里图片全部挂了 🤣

---

## **初始方案：Nginx 反向代理七牛 HTTP 图床**

  

我想走最简单的方式，用自己的服务器做 HTTPS 代理，流量转发给七牛。配置如下：

```
server {
    listen 443 ssl http2;
    server_name qiniussl.iamsb.top;

    ssl_certificate     conf.d/cert/qiniussl.iamsb.top.pem;
    ssl_certificate_key conf.d/cert/qiniussl.iamsb.top.key;
    ssl_session_cache   shared:SSL:10m;
    ssl_session_timeout  10m;
    ssl_protocols        TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass       http://qiniu.iamsb.top;
        proxy_set_header Host            qiniu.iamsb.top;
        proxy_set_header X-Real-IP       $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
    }
}
```

### **✅ 优点**

- 配置简单，不改 PicGo 上传逻辑；
- 不需迁移已有图片。

### **❌ 缺点**

- 我那台小鸡服务器，带宽和性能都拉满；
- 加载一张图要等好几秒，非常影响浏览体验。

---

## **最终方案：找免费 HTTPS 图床 + 自定义 PicGo 插件**

  

我转念一想：干脆省省事，找个**免费又支持 HTTPS**、**API 无限制**的图床，先临时把图托管上去, 后续在找找其他能安全存放图片的方案。后来找到了「[16 图床](https://111666.best/)」，初印象还好那就它了。

  

为了继续保留 PicGo 上传体验，就把 PicGo 稍微改造一下，写了个自定义插件将图片上传到 16图床 上

插件源码地址：

➡️ GitHub: [Layouwen/picgo‑plugin‑custom‑api‑uploader](https://github.com/Layouwen/picgo-plugin-custom-api-uploader)

---

## **总结**

| **阶段** | **方案**                    | **优点**        | **缺点**                     |
| ------ | ------------------------- | ------------- | -------------------------- |
| 初期     | Nginx 反向代理七牛 HTTP 图床      | 简单、迁移无痛       | 服务器性能和带宽受限                 |
| 最终方案   | 免费 HTTPS 图床（16 图床）+ PicGo | 上传体验一致，访问速度还行 | 需要维护 PicGo 自定义插件, 也不知道靠不靠谱 |

若你也正考虑从 Obsidian 搭博客并处理图片问题，希望这篇记录能给你一些参考。欢迎留言讨论～

## 相关链接

[Github 主页](https://github.com/layouwen)

[上文提到到的博客](https://blog.4van.top)

~~[16图床](https://111666.best/)~~ 国内无法访问!!!!

[sm.ms](https://sm.ms/) YYDS!!!!!!!!!!!
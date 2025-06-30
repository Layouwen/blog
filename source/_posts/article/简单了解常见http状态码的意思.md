---
title: 简单了解常见http状态码的意思
date: 2021-07-06 13:50
tags:
  - 博客
  - 前端
  - http
categories:
  - 博客
  - 前端
---

## HTTP 状态码

在日在开发中，前端需要通过后端接口请求数据。在响应中存在标识该资源的状态的状态码。虽然该状态码可以随意修改，但是为了规范化，我们还是需要了解一下常见的状态码意思。

#### 2XX 成功

- 200 成功，表示请求被服务端正确处理
- 204 No Content，表示请求成功，但响应中不含有主体部分
- 205 Reset Content，同 204 一样，但是要求请求方重置内容
- 206 Partial Content，进行范围请求

#### 3XX 重定向

- 301 moved permanently，永久性重定向，表示资源被分配的新的 URL
- 302 found，临时性重定向，表示资源临时被分配了新的 URL
- 303 see other，表示资源存在着另一个 URL，应使用 GET 方法获取资源
- 304 not modified，表示服务器允许访问资源，但因请求未满足条件
- 307 temporary redirect，临时重定向，同 302 类似，但期望客户端保持请求方法不变向新的地址发出请求

#### 4XX 客户端错误

- 400 bad request，请求报文存在语法错误
- 401 unauthorized，表示该请求需要 HTTP 认证信息
- 403 forbidden，表示对请求资源的访问被拒绝
- 404 not found，表示在服务器没有找到请求的资源

#### 5XX 服务器错误

- 500 internal server error，表示服务端在执行请求时发生了错误
- 501 Not Implemented，表示服务器不支持当前请求所需要的某个功能
- 503 service unavailable，表明服务器暂时处于超负荷或正在停机维护，无法处理请求
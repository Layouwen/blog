---
title: HTTP 篇
date: 2021-03-22T22:11:34.000Z
tags:
  - 汇总
  - 知识点总结
  - http
categories:
  - 汇总
  - 知识点总结
uuid: 648df020-0080-461b-a3ab-4c3da94e6a52
---
## 1、HTTP状态码

### 2XX 成功

- 200 OK，客户端的请求被正确处理
- 204 No content，请求成功，响应报文不含实体的主体部分
- 205 Reset Content，请求成功，响应报文不含实体的主体部分，但是需要请求方重置内容
- 206 Partial Content，进行范围请求

### 3XX 重定向

- 301 moved permanently，永久性重定向，资源已分配新的URL
- 302 found，临时性重定向，资源临时分配新的URL
- 303 see other，资源存在另一个URL，应使用GET方法获取资源
- 304 not modified，服务器允许访问资源，请求为满足条件
- 307 temporary redirect，临时重定向，保持请求方法不变，向新地址发送请求

### 4XX 客户端错误

- 400 bad request，请求报文存在语法错误
- 401 unauthorized，需通过HTTP认证的认证信息
- 403 forbidden，对请求资源的访问被服务器拒绝
- 404 not found，服务器没有找到请求资源

### 5XX 服务器错误

- 500 internal sever error，服务端执行请求时发生错误
- 501 Not Implemented，服务器不支持当前请求所需要的某个功能
- 503 service unavailable，服务器处于超负荷或停机维护，无法处理请求

### 总结
2xx 表示成功  
3xx 重定向，表示进一步操作  
4xx 浏览器错误  
5xx 服务器错误

## 2、HTTP缓存

### 强缓存

服务端通知客户端一个缓存时间，在时间内，请求直接使用缓存。不在时间内，则执行比较缓存策略。  

Cache-control（相对值）/ Expries（绝对值）  

Expries是http1.0的标准

```js
let nowTime = new Date()
nowTime.setTime(new Date().getTime() + 3600*1000)
ctx.set("Expires", nowTime.toUTCString())
```

HTTP/1.1，Expire被Cache-control替代

```js
ctx.set("Cache-control", "max-set=3600")
```

1. public：所有内容都被保存（客户端和代理服务器都可缓存）
2. private：所有内容只有客户端可以缓存（默认值）
3. no-cache：客户端缓存内容，是否使用缓存需要经过协商缓存来验证决定
4. no-store：所有内容不会被缓存
5. max-age=xxx：缓存内容将在xxx秒后失效

Cache-Control优先级比Expires高  
from memory cache表示内存中缓存，from disk cache表示硬盘中缓存，读取顺序 memory -> disk

### 协商缓存

命中缓存则返回304状态码，浏览器自动读取缓存内容。如果未命中，则直接返回数据。

Last-Modify/if-Modify-Since：一般设置最后修改时间用于判断  
ETag/if-None-Match：一般设置hash值，如果hash值不一样就返回新的内容  

### 总结

![Image.png](https://i.loli.net/2021/09/07/uVDHOBMexGhbFQd.png)

1. ETag 比较浏览器和服务器资源特征值（如MD5）来决定是否发送数据。如果相同则发送304（not modified）
2. Expires 设置过期时间（绝对时间）如果用户本地时间错乱，会有BUG
3. CacheControl max-age = 3600 设置过期时间（相对时间），与本地时间无关

## 3、GET和POST的区别

1. get用来获取资源，post用来提交资源
2. get可以保存在url地址，post不会保存下来
3. get在回退时不会重新提交，post每次都会重新请求
4. get在url地址中的长度有限制，post基本上没有限制
5. get相比post不安全，get暴露在地址栏中，post在请求体中

## 4、Cookie和LocalStorage和SessionStorage和Session

- cookie 最大4k，保存在浏览器，请求时发送到浏览器
- session 将sessin的id保存在cookie中
- localstorage 容量大5M甚至更多，只有在清理浏览器缓存时才会消失。不会被发送到服务器
- sessionstorage 每次关闭浏览器就会消失

## 5、HTTP请求组成

- 请求行（request line）
    - 请求方法
    - URL
    - 协议版本
- 请求头部（header）
    - 头部字段名
    - 值
- 空行
- 请求数据

## 6、CORS简单请求和复杂请求的区别

Access-Control-Allow-Origin 允许共享给那些域  
Access-Control-Allow-Credentials 凭证标记为true时，是否响应该请求  
Access-Control-Allow-Headers 对预请求响应中，可以使用那些HTTP头  
Access-Control-Allow-Methods 预请求的响应中，那些HTTP方法允许访问资源  
Access-Control-Max-Age 预请求的结果能被缓存多久  
Access-Control-Request-Method 用于发起一个预请求，告知服务器正式请求使用哪一种 HTTP 请求方法  
Origin 指示获取资源的要求是从什么域发起的  

复杂请求会造成两次的请求。
简单请求满足以下条件：
HTTP方法属于其中之一：HEAD、GET、POST
HTTP头信息不超出以下几种字段：
- Accept
- Accept-Language
- Content-Language
- Last-Event-ID
- Content-Type（只能是以下这些）：application/x-www-form-urlencoded、multipart/form-data、text/plain

## 7、http2

- 解析速度快  
http1.1请求时需要不断读入字符串，直到遇到换行（CRLF），http2基于帧协议，每一帧都有表示长度的字段。
- 多路复用  
http1.1发送多个请求需要建立多个TCP连接。  
http2多个请求共用一个TCP连接，同一个请求和响应用一个流表示，有自己唯一的id表示。所以支持乱序发送，到目的地后通过id重新组建。
- 首部压缩
- 

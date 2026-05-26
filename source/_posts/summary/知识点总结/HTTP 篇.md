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
## HTTP状态码

### 2XX 成功

- 200 OK，客户端的请求被正确处理
- 204 No content，请求成功，响应报文不含实体的主体部分
- 205 Reset Content，请求成功，响应报文不含实体的主体部分，但是需要请求方重置内容
- 206 Partial Content，进行范围请求

### 3XX 重定向

- 301 moved permanently，永久性重定向，资源已分配新的URL
- 302 found，临时性重定向，资源临时分配新的URL
- 303 see other，资源存在另一个URL，应使用GET方法获取资源
- 304 not modified，协商缓存命中，服务器返回 304，浏览器继续使用本地缓存
- 307 temporary redirect，临时重定向，保持请求方法不变，向新地址发送请求

### 4XX 客户端错误

- 400 bad request，请求报文存在语法错误
- 401 unauthorized，需认证, 如 未登录/token 失效
- 403 forbidden，拒绝访问, 如 登录了没权限
- 404 not found，服务器没有找到请求资源

### 5XX 服务器错误

- 500 internal sever error，服务端执行请求时发生错误
- 501 Not Implemented，服务器不支持当前请求所需要的某个功能
- 502 Bad Gateway 网关错误, 比较多的就是 nginx 连接不上上游
- 503 service unavailable，服务器处于超负荷或停机维护，无法处理请求

### 总结
2xx 表示成功  
3xx 重定向，表示进一步操作  
4xx 浏览器错误  
5xx 服务器错误

## HTTP缓存

### 强缓存

服务端通知客户端一个缓存时间，在时间内，请求直接使用缓存。不在时间内，则执行比较缓存策略。  

Cache-control（相对值）/ Expries（绝对值）  

Expires 是http1.0的标准

```js
let nowTime = new Date()
nowTime.setTime(new Date().getTime() + 3600*1000)
ctx.set("Expires", nowTime.toUTCString())
```

HTTP/1.1，Expire 被 Cache-control 替代

```js
ctx.set("Cache-Control", "max-age=3600")
```

1. public：所有内容都被保存（客户端和代理服务器都可缓存）
2. private：所有内容只有客户端可以缓存（默认值）
3. no-cache：客户端缓存内容，是否使用缓存需要经过协商缓存来验证决定
4. no-store：所有内容不会被缓存
5. max-age=xxx：缓存内容将在xxx秒后失效

Cache-Control 优先级比 Expires 高  
from memory cache表示内存中缓存，from disk cache表示硬盘中缓存，读取顺序 memory -> disk

### 协商缓存

命中缓存则返回304状态码，浏览器自动读取缓存内容。如果未命中，则直接返回数据。

Last-Modified/If-Modified-Since：一般设置最后修改时间用于判断  
ETag/if-None-Match：一般设置hash值，如果hash值不一样就返回新的内容  

### 总结

![Image.png](https://i.loli.net/2021/09/07/uVDHOBMexGhbFQd.png)

1. ETag 比较浏览器和服务器资源特征值（如MD5）来决定是否发送数据。如果相同则发送304（not modified）
2. Expires 设置过期时间（绝对时间）如果用户本地时间错乱，会有BUG
3. CacheControl max-age = 3600 设置过期时间（相对时间），与本地时间无关
4. ETag 优先级高于 Last-Modified, 且 ETag 比 Last-Modified 准, 前者基于内容生存唯一标识, 后则时间只能精确到秒.

浏览器先检查强缓存：

Cache-Control / Expires

如果命中直接读取缓存。

未命中则进入协商缓存：

ETag / If-None-Match
Last-Modified / If-Modified-Since

如果资源未变化返回304。

## GET和POST的区别

1. get 是幂等的, 用来获取资源，post 通常不是幂等, 用来提交资源.
2. get 在回退时不会重新提交，post每次都会重新请求
3. get 在url地址中的长度有限制(取决浏览器/服务器)，post基本上没有限制
4. get 暴露在地址栏中，post在请求体中. 安全度一样, 只是成本更低, 实际安全依赖 https

## Cookie 和 LocalStorage 和 SessionStorage 和Session 和 IndexedDB

- cookie 最大4k，保存在浏览器，请求时自动发送给服务器
- session 用于服务端保存会话数据, 通过 cookie 带回来.
- localstorage 容量大 5M 甚至更多，只有在清理浏览器缓存时才会消失。不会被发送到服务器. 只能存字符串, 并作为同步 api 大量读写会堵塞主线程.
- sessionStorage tab 级的存储, 关闭标签页清楚. 刷新页面不会丢失, 不同 tab 不共享.
- IndexedDB WebBrower 内置的 NoSQL 结构化数据库系统, 异步 API 支持检索和事务, 可存对象, 离合离线应用大数据缓存. 一般用封装好的库.

## HTTP请求组成

- 请求行（request line）
    - 请求方法
    - URL
    - 协议版本
- 请求头部（header）
    - 头部字段名
    - 值
- 空行
- 请求数据

## 同源策略

协议、域名、端口 全部一致

限制部分:
1. 限制 iframe dom 操作
2. cookie/localstorage/indexeddb 无法互相读取
3. ajax/fetch 请求浏览器默认拦截
4. 跨域 cookie 不会自动携带
不限制部分:
- image
- script
- link
- video/audio

主要用于防止恶意数据读取, 不过只防君子不防小人, 这是浏览器限制服务端不受限制. 服务器已经返回了, 只是浏览器不让 js 读取

## 跨域

- cors
- jsonp
- 反向代理
- postmessage

## CSRF 防御

- SameSite Cookie
- CSRF Token
- Referer 校验

## CORS简单请求和复杂请求的区别

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

## http1

http1 慢的原因
- 队头堵塞, 一个 TCP 只处理一个请求, 会堵塞后面请求.
- TCP 连接多, 为了提高效率一般最多同时 6 个
- Header 重复, 不进行压缩. cookie/token 之类一直重复发送
- 文本解析低效, 基于文本协议. 服务端需要解析字符串和换行, 低于二进制协议.

## http2

- 二进制分帧, 解析速度快 
http1.1请求时需要不断读入字符串，直到遇到换行（CRLF），http2基于帧协议，每一帧都有表示长度的字段。
- 多路复用  
http1.1发送多个请求需要建立多个TCP连接。  
http2多个请求共用一个TCP连接，同一个请求和响应用一个流表示，有自己唯一的id表示。所以支持乱序发送，到目的地后通过id重新组建。
- 首部压缩
- http2 虽然解决了队头堵塞, 但 TCP 层仍有队头阻塞, 后续数据仍要等待重传

核心是: **高并发资源调度能力**
## http3

- http2 依旧基于 TCP, http3 基于 QUIC(UDP + TLS + 多路复用)
- 更快建立连接
- 真正解决队头堵塞
- 更适合弱网移动端

HTTP3/QUIC
本质不是 换了个协议
本质是 针对现代移动互联网高延迟、高丢包场景重新设计传输层。

## JSONP/CORS/跨域

因为浏览器不允许两个源不相同的互相请求。
源=协议+域名+端口号
只要有一样不相同就是不同源。

### 1 CORS

CORS跨源资源共享，它可以解决两个不同源相互访问。
使用方法也很简单，在后端提前设置好Access-Control-Allow-Origin即可。
如果为 * 号则表示可以任何人访问。
如果需要指定源访问，可以在后面接上允许的源地址。
比如：Access-Control-Allow-Origin：http://foo.example

### 2 JSONP

JSONP也是一种跨域解决方案，由于IE之前的版本不支持CORS进行跨域。所以我们需要想别的解决思路，就出现了JSONP。它与JSON没有关系，之所以这样叫是因为最初里面数据写的格式大多为JSON格式。

JSONP原理

`<script src="">` 不受同源策略限制.
核心是动态 script 注入.

1. 前端定义一个 callback 函数
2. 通过 script 请求, 并等后端返回基于 callback 函数的拼接.
3. 前端获取返回后执行该函数, 获取数据

- 返回可执行 js 容易 xss 风险
- 错误处理能力差

### 3 sessionStorage和localstorage能跨域拿到吗？

localstorage只有在同源的情况下可以获取数据
sessionStorage不同页面和标签页面无法共享数据

#### 3.1 localStorage跨域二级域名共享方法

挂载一个隐藏的iframe加载顶级域名的代理页面，统一在顶级域名里操作localStorage，其他二级或n及域名读取或设置localStorage通过此ifarme来操作

顶级域名的html
```html
<script>
    document.domain = 'lyw.com' // 设置顶级域名
    const setItem = (key, data) => localStorage.setItem(key, data)
    const getItem = key => localStorage.getItem(key)
</script>
```

其他二级或n级域名
```html
<iframe style="display:none;" id="ifrStorageProxy" src="http://www.lyw.com/storageproxy.html"></iframe>
<script>
  document.domain = 'lyw.com'
  const winifrStorageProxy = document.getElementById('ifrStorageProxy').contentWindow
  winifrStorageProxy.onload = () => {
    winifrStorageProxy.setItem('showbo', 'test-' + newDate().toLocaleString()
    alert(winifrStorageProxy.setItem('showbo'))
  }
</script>
```

#### 3.2 sessioniStorage和localStorage跨顶级域名共享方法

与二级域名类似，任选一个域名挂载iframe代理页面，数据存处在此顶级域名下，其他顶级域名通过此代理读取localStorage和sessionStorage的数据

代理的html
```html
<script>
  window.addEventListener('message', e =>{
    if (e.source !== parent) return // 如果不是父页面发送的信息就返回
    let data = JSON.parse(e.data)
    switch (data.action) {
      case 'setItem':
        localStorage.setItem(data.key, data.data)
        break
      case 'getItem':
        data = localStorage.getItem(data.key)
        parent.postMessage(data, '*')
        break
    }
  }, false)
<script>
```

其他顶级域名页面
```html
<iframe style="display: none;" if="ifrStorageProxy" src="http://www.lyw.com/storageproxy.html"></iframe>
<script>
  let winifrStorageProxy = document.getElementById('ifrStorageProxy')
  window.addEventListener('message', e => {
    if (e.soure !== winifrStorageProxy.contentWindow) return // 不是我们自己iframe发送的信息就返回
    alert(e.data)
  }, false)
  // 要在加载完毕后进行操作，不然onmessage为注册
  winifrStorageProxy.onload = () => {
    // 发送存储指令
    winifrStorageProxy.contentWindow.postMessage(
      JSON.stringify(
        { 
          action: 'setItem', 
          key: 'showbo', 
          data: new Date().toLocaleString()
        }
    ), '*')
    // 发送读取指令
    winifrStorageProxy.contentWindow.postMessage(JSON.stringify({
      action: 'getItem',
      key: 'showbo'
    }), '*')
  }
</script>
```
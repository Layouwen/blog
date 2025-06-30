---
title: 跨域CORS、JSONP、服务器中转
date: 2020-06-11 18:14
tags:
  - 博客
  - 后端
  - nodejs
categories:
  - 博客
  - 后端
---

## 1、同源策略


当两个服务器的 源 不完全相同的时候，无法获取另一个的数据。不同的页面，无法相互访问数据。



### 1.1 获取网站的源


```javascript
window.origin
或
location.origin
```



### 1.2 判断源是否相同


源 = 协议 + 域名 + 端口号



> 当协议、域名、端口号（完全一致）才是同源，否者都为不同源
>
> [http://baidu.com](http://baidu.com) 与 [http://www.baidu.com](http://www.baidu.com) 不同源
>
> [http://baidu.com](http://baidu.com) 与 [http://qq.com](http://qq.com) 不同源
>



## 2、跨域


为了解决同源策略无法相互访问数据



其中两种跨域的方法：CORS、JSONP



### 2.1 CORS


在后端设置响应头，提前声明允许谁获取数据。



```javascript
response.setHeader("Access-Control-Allow-Origin", "允许的源地址，例如（http://baidu.com）")
```



> 优点：操作简单
>
> 缺点：不兼容IE 6 7 8 9
>



### 2.2 JSONP


将数据写入到JS文件中，利用引用JS文件，将数据保存到window上。最后再利用window来获取JS保存的数据。



> 优点：兼容IE、可以跨域
>
> 缺点：因为是通过js的script获取的，所以他不支持post，只可以发get请求。以及没有AJAX那样获取精确的状态码等数据。
>



### 2.3 服务器中转


使用自己的服务器作为中转站，将需要请求的地址，发送给服务端。服务端不存在跨域问题，可以直接请求数据。返回给我们自己。



中转服务器



```javascript
const http = require('http')
const url = require('url')
http.createServer((req, res) => {
  let urlObj = url.parse(req.url, true)
  if (urlObj.pathname === '/bridge') {
    http.get(urlObj.query.url, req => {
      let text = ''
      req.on('data', data => text += data)
      req.on('end', () => {
        res.setHeader('Access-Control-Allow-Origin', '*')
        res.end(text)
      })
    })
  } else {
    res.writeHead(404, 'Not Found')
    res.end('not found')
  }
}).listen(8080)
```



浏览器正常请求即可



```javascript
fetch('http://localhost:8080/bridge?url=' + encodeURIComponent('http://baidu.com'))
  .then(res => res.text())
  .then(data => console.log(data))
```



## 3、JSONP封装


### 3.1 方案1


```javascript
function jsonp(url) {
    return new Promise((resolve, reject) => {
        const random = 'JSONCallbackName' + Math.random()
        window[random] = data => {
            resolve(data)
        }
        const script = document.createElement('script')
        script.src = `${url}?callback=${random}`
        script.onload = () => {
            script.remove()
        }
        script.onerror = () => {
            reject()
        }
        document.body.appendChild(script)
    })
}
jsonp('访问数据的文件地址').then((data) => {
    console.log(data)
})
```



### 3.2 方案2


服务端



```javascript
const http = require('http')
const url = require('url')
http.createServer((req, res) => {
  let urlObj = url.parse(req.url, true)
  if (urlObj.pathname === '/getWeather') {
    let data = { city: 'guangzhou', weather: 'sunny' }
    res.end(`${urlObj.query.callback}(${JSON.stringify(data)})`)
  } else {
    res.writeHead(404, 'Not Found')
    res.end('not found')
  }
}).listen(8080)
```



客户端



```javascript
function jsonp(url, data = {}) {
  return new Promise((resolve, reject) => {
    window.__jsonp__ = data => resolve(data)
    let script = document.createElement('script')
    let query = Object.entries(data).map(a => `${a[0]}=${a[1]}`).join('&')
    script.src = url + '?callback=__jsonp__&' + query
    script.onerror = () => reject('加载失败')
    document.head.appendChild(script)
    document.head.removeChild(script)
  })
}

jsonp('http://api.layouwen.com/getWeather.php', { city: '广州' }).then(data => {
  console.log(data)
}).catch(e => console.log(e))
```


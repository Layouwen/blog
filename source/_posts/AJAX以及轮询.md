---
title: AJAX以及轮询
date: 2021-01-30 17:02
tags:
  - 博客
  - 前端
  - ajax
  - 轮询
categories:
  - 博客
  - 前端
---

## 1. 传统表单提交与AJAX比较

### 1.1 Form表单

```html
<form action="/form.html" method="post">
  <input type="text" name="username" placeholder="username" />
  <input type="password" name="password" placeholder="password" />
  <input type="submit" />
</form>
```

- 它支持持GET和POST类型
- 提交数据后页面会刷新，用户体验差

### 1.2 AJAX

- 概念
**AJAX** = **Asynchronous JavaScript and XML** （异步的JavaScript 和 XML）
- 使用内置的 `XMLHttpRequest` 和 `fetch` 对象，实现和服务器的交互
- 交互数据时不需要刷新数据，用户体验好

#### 1.2.1 XMLHttpRequest

##### GET 请求

- 创建对象
- 配置参数
- 绑定事件
- 发送请求

```js
let xhr = new XMLHttpRequest()
xhr.open('GET', url, true)
xhr.onload = function(){}
xhr.send()
```

方法1

```js
let url = 'http://xxx.xxx.com/api/blog/list'
let xhr = new XMLHttpRequest()
xhr.open('GET', url, true)
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
      console.log(JSON.parse(xhr.responseText))
    } else {
      console.log('数据错误')
    }
  }
}
xhr.onerror = () => {
  console.log('网络异常')
}
xhr.send()
```

方法2

```js
let url = 'http://xxx.xxx.com/api/blog/list'
let xhr = new XMLHttpRequest()
xhr.open('GET', url, true)
xhr.onload = () => {
if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
  console.log(xhr.response)
} else {
  console.log('数据错误')
}
}
xhr.onerror = () => {
  console.log('网络异常')
}
xhr.send()
```

##### POST 请求

```js
let url = 'http://xxx.xxx.com/api/user/login'
let xhr = new XMLHttpRequest()

xhr.timeout = 3000 // 设置请求超时时间
xhr.open('POST', url, true)
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
xhr.onload = () => {
  if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
    console.log(JSON.parse(xhr.responseTest))
  } else {
    console.log('数据错误')
  }
}
xhr.ontimeout = () => {
  console.log('请求超时')
}
xhr.onerror = () => {
  console.log('网络异常')
}
xhr.send('username=layouwen&password=123456')
```

简单的封装

```js
const request = (url, params, onsuccess, onerror) => {
  url = url + '?' + Object.entries(params).map(arr=>arr[0] + '=' + arr[1]).join('&')
  let xhr = new XMLHttpRequest()
  xhr.open('POST', url, true)
  xhr.onload = () => {
    if (xhr.status === 200 || xhr.status === 304) {
      onsuccess(JSON.parse(xhr.responseText))
    } else {
      onerror()
    }
  }
  xhr.onerror = onerror
  xhr.send()
}

request('http://www.xxx.com/api/user/login', {
  username: 'layouwen',
  passowrd: '123456'
}, data => {
  console.log('请求成功', data)
}, () => {
  console.log('网络异常')
})
```

#### 1.2.2 Post请求编码方式

两个常用的

application/x-www-form-urlencoded
- 参数变为 `key1=value1&key2=value2` 的形式

multipart/form-data
- 一般用于上传文件使用

##### multipart/form-data

```js
let formData = new FormData()
formData.append('username', 'layouwen')
formData.appedn('password', '123456')
let url = 'http://www.xxx.com/api/user/login'
let xhr = new XMLHttpRequest()
xhr.open('POST', url, true)
xhr.onload = () => {
  if (xhr.status === 200 || xhr.status === 304) {
    console.log(JSON.parse(xhr.responseText))
  } else {
    console.log('接口异常')
  }
}
xhr.send(formData)
```

#### 1.2.3 fetch

##### GET

```js
let url = 'http://www.xxx.com/api/blog/list'

fetch(url).then(response => response.json()).then(data => {
  document.body.innerText = data.results[0].weather_data[0].weather
})
```

##### POST

```js
let url = 'http://www.xxx.com/api/user/login'
let data = {
  username: 'layouwen',
  password: '123456'
}

fetch(url, {
  method: 'POST',
  body: Object.entries(data).map(arr=>arr[0] + '=' + arr[1]).join('&')
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
}).then(res=>res.json()).catch(error => {
  console.log('Error', error)
}).then(response => {
  console.log('Success', response)
})
```

> 更多用法 [https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)

## 2. 双工通信

### 2.1 轮询

每隔一段时间发一次请求
- 发请求，立即响应，返回空
- 发请求，立即响应，返回新数据
- 发请求，立即响应，返回数据为空
- 发请求，立即响应，返回数据为空
......

> gitee案例 `chat-xhr-poll`

### 2.2 长轮询（Comet）

客户端
- 发请求，等待响应
- 当响应时，再次发送请求

服务端
- 请求到来，如果没新数据，则不发
- 当有新数据，需要通知客户端，再响应

> gitee案例 `chat-comet`

### 2.3 WebSocket

> gitee案例 `chat-websocket`
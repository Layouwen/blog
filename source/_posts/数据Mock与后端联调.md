---
title: 数据Mock与后端联调
date: 2021-01-30 15:18
tags:
  - 博客
  - 前端
  - mock
  - nodejs
categories:
  - 博客
  - 前端
---

## 1、前言

通常前端开发的时候，我们的页面已经开发完成。但是后端的接口不一定已经写完就了。这时候我们就可以自己去伪造一些符合规范的数据，用于前期的测试，等到后端接口完成的时候我们可以在进行一个接口联调。

## 2、Mock数据的方法

### 2.1 自己手写一个简易服务器返回数据

对于我们前端来说，我们天生就会一个后端开发语言，nodejs即可实现。

我们只需要创建一个简易服务器，对请求路径进行判断，返回对应的mock数据。

```js
const http = require('http')
const fs = require('fs')
const url = require('url')

http.createServer((req, res) => {
  let pathObj = url.parse(req.url, true)
  switch (pathObj.pathname)
    case '/getWeather':
      if (pathObj.query.city === 'beijing') {
        res.end(JSON.stringify({city: 'beijing', weather: 'sunny'}))
      } else {
        res.end(JSON.stringify({city: pathObj.query.city, weather: 'unknown'}))
      }
      braak
    default:
      try {
        let pathname = pathObj.pathname === '/' ? '/index.html' : pathObj.pathname
        res.end(fs.readFileSync(__dirname + pathname))
      } catch(e) {
        res.writeHead(404, 'Not Found')
        res.end('<h1>404 Not Found</h1>')
      }
}).listen(8080)
```

> 上面这个只是简单的返回，你完全可以使用Express等框架，搭建一个更好用的Mock服务器

### 2.2 使用Mock.js和Mock平台

#### 2.2.1 Mock.js

Mock.js可以快速通过模板生成数据。

[http://mockjs.com/examples.html](http://mockjs.com/examples.html)

1. 常见用法

`@ctitle(3, 10)`
`@cparagraph`
`@cword`
`@cname`
`@integer(10, 100)`
`@float(20, 30, 2, 3)`
`@color`
`@date`
`@time`
`@now`
`@id`
`@url`
`@email`
`@image('200x100')`

2. 使用例子

```js
{
  'statusCode| 1': [1, 3, 2, 8],
  'msg': '@cword(4, 10)',
  'data| 4': [
    {
      id: '@id',
      title: '@ctitle',
      author: '@cname',
      createdAt: '@datetime'
    }
  ]
}
```

#### 2.2.2 Rap2

[http://rap2.taobao.org](http://rap2.taobao.org)

> 这个平台就是类似在线的服务器，加上Mock.js。实现通过他提供的接口，可以自定义返回相应的数据。

## 3、接口规范

### 3.1 接口约定

约定好接口的路径是什么？
如
`/auth/register`

接口的提交类型是什么？
如
`GET` 获取数据
`POST` 提交或创建
`PATCH` 修改数据，部分修改
`DELETE` 删除数据
`PUT` 修改数据，整体替换原有数据

参数类型/格式
如
`fromdata` 或者 `application/x-www-form-urlencoded`

参数字段限制条件
返回成功的格式
返回失败的格式

### 3.2 接口测试

当后端给到你接口的时候，你可以使用命令行的 `curl` 语句，进行简单的测试。

```js
// GET 请求
curl "http://xxx.xxx.com/api/blog/getBlog?id=1"

// -d 提交的参数，默认是POST
curl -d "username=layouwen&password=12345" "http://xxx.xxx.com/api/user/login"

// -i 展示响应头
curl -d "username=layuouwen&password=12345" "http://xxx.xxx.com/api/user/login" -i

// -H 设置请求头
curl -H "Content-Type:application/json" -X POST -d '{"user": "layouwen", "password": "123456"}' "http://xxx.xxx.com/api/user/login"

// -X 设置请求类型
curl -d "username=layouwen&passowrd=bbb" -X POST "http://xxx.xxx.com/api/blog/list"

// -b 请求带上cookie
curl "http://xxx.xxx.com/api/user/login" -b "connect.sid=df1431 35f89a7sdf89gasdf2g#$@123."
```
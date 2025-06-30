---
title: 使用Node.js和JSON搭建简单的动态服务器
date: 2020-06-14 10:30
tags:
  - 博客
  - 后端
  - nodejs
categories:
  - 博客
  - 后端
---

## 一、创建html页面

创建4个页面，index.html、register.html、sign_in.html、home.html

- index.html 默认主页
- register.html 用于注册账号
- sign_in.html 用于登录账号
- home.html 用于显示登录后的页面

### 主要代码片段

register.html

```html
<form id="registerForm">
    <div>
        <label for="">用户名：<input type="text" name="name" id=""></label>
    </div>
    <div>
        <label for="">密码：<input type="password" name="password" id=""></label>
    </div>
    <div>
        <button type="submit">注册</button>
    </div>
</form>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
let $form = $('#registerForm')
$form.on('submit', (e) => {
    e.preventDefault()
    const name = $form.find("input[name=name]").val()
    const password = $form.find('input[name=password').val()
    console.log(name, password)
    // pass AJAX post data
    $.ajax({
        method: 'post',
        url: '/register',
        contentType: 'text/json; charset=UTF-8',
        data: JSON.stringify({
            name, // name: name 
            password // password: password
        })
    }).then(() => {
        alert('注册成功')
        location.href = '/sign_in.html'
    }, () => {})
})
</script>
```

sign_in.html

```html
<form id="signInForm">
    <div>
        <label for="">用户名：<input type="text" name="name" id=""></label>
    </div>
    <div>
        <label for="">密码：<input type="password" name="password" id=""></label>
    </div>
    <div>
        <button type="submit">登录</button>
    </div>
</form>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script>
    let $form = $('#signInForm')
    $form.on('submit', (e) => {
        e.preventDefault()
        // get name password
        const name = $form.find("input[name=name]").val()
        const password = $form.find('input[name=password').val()
        // pass AJAX post data
        $.ajax({
            method: 'POST',
            url: '/sign_in',
            contentType: 'text/json; charset=UTF-8',
            data: JSON.stringify({
                name,
                password
            })
        }).then(() => {
            alert('登录成功')
            location.href = '/home.html'
        }, () => {})
    })
</script>
```

home.html

```html
<p>
    {{loginStatus}}
</p>
<p>
    你好,{{user.name}}
</p>
<p>
    <a href="sign_in.html">登录</a>
</p>
```

## 二、Node服务器

```js
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if (!port) {
    console.log('请输入指定端口。如：\nnode server.js 8888')
    process.exit(1)
}

var server = http.createServer(function (request, response) {
    var parsedUrl = url.parse(request.url, true)
    var pathWithQuery = request.url
    var queryString = ''
    if (pathWithQuery.indexOf('?') >= 0) {
        queryString = pathWithQuery.substring(pathWithQuery.indexOf('?'))
    }
    var path = parsedUrl.pathname
    var query = parsedUrl.query
    var method = request.method

    /******** main start ************/
    // 读取 session 文件,转化为对象
    const session = JSON.parse(fs.readFileSync('./session.json').toString())

    if (path === '/sign_in' && method === 'POST') {
        // 读数据库
        let userArray = JSON.parse(fs.readFileSync('./database/users.json'))
        const array = []
        // 每次接受数据就添加进数组
        request.on('data', (chunk) => {
            array.push(chunk)
        })
        request.on('end', () => {
            // 转化字符串
            const string = Buffer.concat(array).toString()
            // 在转化为对象
            const obj = JSON.parse(string)
            // 找到符合的 user
            const user = userArray.find(user => user.name === obj.name && user.password === obj.password) // 成功返回符合的对象，失败返回undefined
            if (user === undefined) { // 失败
                response.statusCode = 400
                response.setHeader('content-Type', 'text/JSON; charset=UTF-8')
                response.end(`{"errorCode":4001}`)
            } else { // 成功
                response.statusCode = 200
                // 设置 Cookie
                const random = Math.random()
                session[random] = {
                    user_id: user.id
                }
                // 写入数据
                fs.writeFileSync('./session.json', JSON.stringify(session))
                response.setHeader("Set-Cookie", `'session_id=${random}; HttpOnly'`)
                response.end()
            }
        })
    } else if (path === '/home.html') {
        // 获取 Cookie
        const cookie = request.headers['cookie']
        let sessionId
        try { // 读取 Cookie 中的 id 值
            sessionId = cookie.split(';').filter(s => s.indexOf('session_id=') >= 0)[0].split('=')[1]
        } catch (error) {}
        if (sessionId && session[sessionId]) {
            // 从 session 中读取对应的值
            const userId = session[sessionId].user_id
            // 读数据库
            let userArray = JSON.parse(fs.readFileSync('./database/users.json'))
            // 找到符合的 user
            let user = userArray.find(user => user.id === userId)
            const homeHtml = fs.readFileSync('./public/home.html').toString()
            let string
            if (user) {
                string = homeHtml.replace('{{loginStatus}}', '已登录').replace('{{user.name}}', user.name)
                response.write(string)
            }
        } else {
            // 读取源文件内容
            const homeHtml = fs.readFileSync('./public/home.html').toString()
            // 替换文字
            const string = homeHtml.replace('{{loginStatus}}', '未登录').replace('{{user.name}}', '')
            response.write(string)
        }
        response.end()
    } else if (path === '/register' && method === 'POST') {
        response.setHeader('Content-Type', 'text/html; charset=UTF-8')
        // read database
        let userArray = JSON.parse(fs.readFileSync('./database/users.json')) // read database
        const array = []
        request.on('data', (chunk) => {
            array.push(chunk)
        })
        request.on('end', () => {
            // convert string
            const string = Buffer.concat(array).toString()
            // convert obj
            const obj = JSON.parse(string)
            // last user id
            const lastUser = userArray[userArray.length - 1]
            // new user
            const newUser = {
                id: lastUser ? lastUser.id + 1 : 1,
                name: obj.name,
                password: obj.password
            }
            userArray.push(newUser)
            // write data
            fs.writeFileSync('./database/users.json', JSON.stringify(userArray))
        })
        response.end()
    } else {
        response.statusCode = 200
        let content
        // setting index
        const filePath = path === '/' ? '/index.html' : path
        // judge type
        const index = filePath.lastIndexOf('.')
        const suffix = filePath.substring(index)
        const fileType = {
            '.html': 'text/html',
            '.css': 'text/css',
            '.js': 'text/javascript'
        }
        response.setHeader('Content-Type', `${fileType[suffix] || "text/html"};charset=utf-8`)
        try {
            content = fs.readFileSync(`./public${filePath}`)
        } catch (error) {
            content = '文件路径不存在'
            response.statusCode = 404
        }
        response.write(content)
        response.end()
    }

    /******** main end ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功！请输入下列地址访问\nhttp://localhost:' + port)
```

## 三、主要思路

### register.html

使用jQuery的ajax将数据发送请求 /register 给后端，成功则跳转到 sign_in.html

> 数据需要使用 JSON.stringify 转化为字符串在提交

### /register

读取 users.json 的数据，创建一个空数组，将传递过来的参数 push 进去。将数组转换为字符串，在转换为对象。
获取数据库中最小的 id 值，将数据组成新的对象，添加进入 数据库 中。

### sign_in.html

使用ajax将数据发送请求 /sign_in 给后端，成功则跳转 home.html

### /sign_in

读取 users.json 的数据，创建一个空数组，将传递过来的参数 push 进去。将数组转换为字符串，在转换为对象。
在读取后的数据库中，查找有没有符合条件的 user，成功返回读取后的对象，失败返回 undefined。
如果成功，设置随机数，将 随机数的值 与 user的id 绑定。并添加到 session.json 中。然后 setHeader，将cookie发送到浏览器。

### /home

获取登入成功后 cookie 的值。读取 session 中对应的随机数。如果随机数和session对应的随机数值存在，就显示已登录，否则显示未登录
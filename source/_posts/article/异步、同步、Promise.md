---
title: 异步、同步、Promise
date: 2020-06-10 14:22
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

## 一、同步


当你执行时，能直接获得结果就是同步。



> 当有个任务需要等待很久的时候，下一个任务将无法运行。大量浪费时间。
>



## 二、异步


当一个函数不能直接直接拿到结果，可以先执行其他代码，等待获取结果后再返回，就是异步。



异步两种形式：



+ 轮询  
定时的去询问他获取了数据没有
+ 回调  
等结果获取后，在回头调用



> 异步的好处：它可以用等待的时间去执行其他事情
>



### 1.回调


写了个A函数，不立刻执行。交给B函数，回头调用。这就是回调。



> 回调可以用于同步任务，并非只能异步任务
>
> 回调还可以将函数传给浏览器进行调用。如：request.onreadystatechange
>



### 2.获取异步的结果


当异步有两个结果时可以采用以下方法获取结果



方法一：回调接受两个参数



```javascript
fs.readFile('./文件.txt', (error, data)=>{
    if(error){ console.log('失败');return }
    console.log(data.toString())
})
```



方法二：用两个回调接受



```javascript
ajax('get', '/文件.json', data=>{}, error=>{})
```



或



```javascript
ajax('get', '/文件.json', {
    success: ()=>{},
    fail: ()=>{}
})
```



> 上面两种方法都有很大的缺陷
>
> 命名不规范，五花八门
>
> 容易出现回调地狱，很容易让人看不懂
>
> 很难对错误进行处理
>



## 三、判断同步&异步


有些函数默认就是异步：如



+ setTimeout
+ AJAX（即XMLHttpRequest）
+ addEventListener



> AJAX可以设置为同步，不过没什么作用。如：request.open('参数1', '参数2', false)
>



## 四、Promise


Promise可以有效解决异步对结果的处理。



> 规范的命名及顺序
>
> 没有了回调地狱，可读性更好
>
> 方便捕获错误
>



以AJAX封装为例，如果用正常的方法来获取结果，代码如下：



```javascript
ajax = (method, url, options)=>{
    const {success, fail} = options // 析构赋值
    const request = new XMLHttpRequest()
    request.open(method, url)
    request.onreadystatechange = ()=>{
        if(request.readystate === 4){
            // 成功调用success 失败调用fail
            if(request.status < 400){
                success.call(null, request.response)
            }else if(request.status >= 400){
            fail.call(null, request, request.status)
            }
        }
    }
    request.send()
}

ajax('get', '/xxx', {
    success(respone){}, fail: (request, status)=>{}
})
```



使用Promise的方法，代码如下：



```javascript
ajax('get', '/xxx')
    .then((response)=>{}, (request, status)=>{})
})
// 第一个参数是success 第二个参数是fail
```



> ajax() 返回了含有 .then() 方法的对象
>



```javascript
ajax = (method, url, options)=>{
    return new Promise((resolve, reject)=>{
        const {success, fail} = options
        const request = new XMLHttpRequest()
        request.open(method, url)
        request.onreadystatechange = () => {
            if(request.status < 400){
                resolve.call(null, request.response)
            }else if(request.status >= 400){
                reject.call(null, request)
            }
        }
        request.send()
    })
}
```



> 核心代码 return new Promise((resolve, reject)=>{})
>
> 代码执行成功调用 resolve(result)
>
> 代码执行失败调用 reject(error)
>
> .then(success, fail) 传入成功和失败的函数
>



## 五、AJAX库


1. jQuery.ajax  
[链接](https://www.jquery123.com/)
2. Axios  
[链接](http://www.axios-js.com/)


---
title: JS函数setTimeout的小细节
date: 2020-03-19 22:32
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

首先看一个代码
```javascript
let i
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```
上的代码看起来只是简单的for循环输出i的值。但是当你运行上面代码，你会发现，竟然输出了6个6。

那么我们开始分析这个过程。
setTimeout是个计时函数，后面的0代表着0毫秒后执行。
我们简单的可以理解为，它是等一下才执行。那么这个等一下是多久呢。
我们先运行一下正常的for循环consloe一下数值i

```javascript
let i
for(i = 0; i<6; i++){
  console.log(i)
}
```
经过测试，你会发现他正常的输出了0~5的数值。那么6怎么来的呢？
经过思考可以猜到。for最后一次循环，他判断5<6符合条件，他就执行了后面的i++。所以for循环的值最后变成了6。
那么我们知道了6是for循环结束后才出现的。我们可以得出结论

```javascript
let i
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```

这里面的setTimeout他会等for循环运行完后才运行自身的代码。

为了解决这种尴尬的问题，我们可以使用下面两种方法来实现每次循环都正常执行setTimeout。

方法1:

在for循环内添加一个变量，来接收每次for循环的值。

```javascript
let i
for(i = 0; i<6; i++){
  let j = i
  setTimeout(()=>{
    console.log(j)
  },0)
}
```

方法2:
在for里面声明变量i
```javascript
for(let i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```

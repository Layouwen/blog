---
title: JavaScript预解析
date: 2020-05-15 11:14
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

## 预解析
JavaScript引擎运行时，分两步
1. 预解析
JavaScript会将 var 和 function 优先解析
    - 变量提升
只提升变量，不提升赋值
    - 函数提升
只提升声明，不调用函数
2. 代码执行
预解析结束后，按代码从上往下执行

> 函数内部变量如果没有声明，直接赋值。则为全局变量

### 测试1

```js
var num = 10
fun()
function fun() {
    console.log(num)
    var num = 20
}
```

结果为：
```js
undefined
```

解析：
它的执行过程是
```js
function fun() {
    var num
    console.log(num)
    num = 10
}
var num = 10
fun()
```

### 测试2

```js
var num = 10
function fn() {
    console.log(num)
    var num = 20
    console.log(num)
}
fn()
```

结果为：
```js
undefined
20
```

解析：
它的执行过程是
```js
var = num
function fn() {
    var num
    console.log(num)
    num = 20
    console.log(num)
}
num = 10
fn()
```

### 测试3

```js
var a = 18
f1()
function f1() {
    var b = 9
    console.log(a)
    console.log(b)
    var a = '123'
}
```

结果为：
```js
undefined
9
```

解析：
它的执行过程是
```js
var a
function f1() {
    var b
    var a
    b = 9
    console.log(a)
    console.log(b)
    a = '123'
}
a = 18
f1()
```

### 测试4

```js
f1()
console.log(c)
console.log(b)
console.log(a)
function f1() {
    var a = b = c = 9
    console.log(a)
    console.log(b)
    console.log(c)
}
```

结果为：
```js
9
9
9
9
9
报错
```

解析：
它的执行结果为
```js
function f1() {
    var a 
    a = 9
    b = 9
    c = 9
    console.log(a)
    console.log(b)
    console.log(c)
}
f1()
console.log(c)
console.log(b)
console.log(a)
```
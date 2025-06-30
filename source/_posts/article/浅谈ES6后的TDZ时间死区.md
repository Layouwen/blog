---
title: 浅谈ES6后的TDZ时间死区
date: 2021-01-27 09:45
tags:
  - 博客
  - 前端
  - javascript
  - tdz
categories:
  - 博客
  - 前端
---

## 1、const、let和var

在es6新特特性这，let和const可以达到局部作用域的效果。

### 1.1 var

```js
var a = 4
function say () {
  console.log(a)
  var a
}
say()
```

大家在网上听的最多的就是var会进行变量提升，事实也是如此。上面例子中，函数外部声明变量a赋值为4。当调用函数时，内部的变量a会提到log前提前声明，并且默认赋值为undefined。然后才log。如下图

```js
var a = 4
function say () {
  var a = undefined
  console.log(a)
}
say()
```

所以你们看到的结果为undifind，同理下面例子也是如此

```js
console.log(b)
var b = 4
console.log(b)
```

上面例子会先变量声明赋值为undefined，然后在将4赋值给变量b

```js
var b = undefined
console.log(b)
b = 4
console.log(b)
```

### 1.2 let/const

在let中，很多人在面试中都会回答到。他与var最大的区别是不会变量提升，其实不然。可以看下面这个例子

```js
let a = 1
function say () {
  console.log(a)
  let a = 2
}
say()
```

按照大多数人的了解，这个结果不会变量提升，所以应该输入外部的1才对。但是结果却是报错，下面看看分析。

```js
let a = 1
function say () {
  let a
  console.log(a)
  a = 2
}
```

在调用say函数的时候，他发现在局部作用域（函数内部）里有一个变量声明，它依旧会将其变量提升，只是这次与var的区别不太一样，var在提升的同时，他会进行一个赋值为undefined，但是let只是单纯的变量提升。所以你会发现这个是报错的。

> const 也是一样的道理，只是在const中不能对变量进行更改。

## 2、TDZ时间死区

其实刚刚我们已经接触到了时间死区，只是你没有发现。

```js
let a = 520
function say () {
  // 时间死区开始
  console.log(a)
  // 时间死区结束
  let a = 520
}
```

在上面例子中，因为我们下方使用let对a进行变量声明后，造成的变量提升，导致我们无法对a进行修改。这个位置就是我们说的时间死区。
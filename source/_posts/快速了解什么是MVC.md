---
title: 快速了解什么是MVC
date: 2020-03-15 16:34:23
tags:
  - 前端
categories:
  - 博客
  - 前端
---

- MVC设计模式
- 简单认识EvenBus
- 表驱动编程
- 模块化

## 一、MVC设计模式

### 1. 为什么要有设计模式

代码重复太多，相同页面重复写等。MVC设计模式就是为了解决代码重复，优化代码结构。

### 2. 学MVC会有什么情况

- 会出现意大利面条式代码的情况，过于臃肿太长，代码重复率高，页面不整洁等。
- 你会变成类似外包式程序员，每天重复干着重复的事情，不懂得提升自己，不会封装，不会造轮子。

### 3. 什么是MVC

将平时全部堆在一起的代码，进行分类管理，细分成一段段小代码。再将代码分种类，分别放入对应M模块、V模块、C模块。方面自己和他人阅读、开发。

#### M —— Model（数据类型）负责操作所有数据

```javascript
// 数据相关放入m中
const m = {
    数据1,
  数据2,
  数据3
}
```

#### V —— View（视图）负责所有UI页面

```javascript
// 视图相关放入v中
const v = {
    html代码,
    渲染等
}
```

#### C —— Controller（控制器）负责其他

```javascript
// 其他全部放入c中
const c = {
    事件,
  添加,
  修改等
}
```

## 二、简单识EvenBus

### - 什么是EvenBus

EvenBus可以使用监听和触发事件，对所点实现通信。

### - 监听和触发的两个API

```javascript
// 发送一个信息，用于给监听事件监听
eventBus.trigger('我触发了')
// 监听一个信息，若监听到了则执行
eventBus.on('我触发了', 执行语句)
```

### - EvenBus伪代码事例

声明eventBus

```javascript
const eventBus = $(window)
```

创建监听和触发事件

```javascript
组件1 {
    ···
  eventBus.trigger('hereTrigger')
  ···
}
组件2 {
    ···
  eventBus.on('hereTrigger'，()=>{
    consolo.log('Receive message')
  })
  ···
}
```

eventBus.trigger执行后，会把hereTrigger发送出去。

如果evenBus.on收到了信息与hereTrigger一致，那么就会执行consolo.log。

如果eventBus.trigger没有执行，那么eventBus.on将不会执行。

## 三、表驱动编程

### - 定义

表驱动编程，又称之为表驱动、表驱动方法。 “表”是几乎所有数据结构课本都要讨论的非常有用的数据结构。表驱动方法出于特定的目的来使用表，程序员们经常谈到“表驱动”方法，但是课本中却从未提到过什么是"表驱动"方法。表驱动方法是一种使你可以在表中查找信息，而不必用很多的逻辑语句（if或Case）来把它们找出来的方法。事实上，任何信息都可以通过表来挑选。在简单的情况下，逻辑语句往往更简单而且更直接。但随着逻辑链的复杂，表就变得越来越富有吸引力了。

简单来说，将数据统计放在一个位置，需要获取里面数据的时候，像表一样，一一对应。

### - 简单的例子

拿最常见的if语句来举例

```javascript
if(key = 'A'){
    执行语句a 
}else if(key = 'B'){
    执行语句b
}else if...
```

改为表驱动编程来实现

```javascript
let table = {
    a: {
    action () {
        console.log("执行语句a")
    }
  },
    b: {
    action () {
        console.log("执行语句b")
    }
  }
}

function handleTable(key) {
  return table[key]
}
handleTable('a').action()
```

## 四、模块化

所谓模块化，就是一种将复杂代码分解为更好的可管理模块的方式。MVC设计模式就是一种模块化的体现，将代码分门别类的进行管理。

### - 各自独立

在平时写代码的时候，可以将不同功能的代码，单独一个文件来写，然后通过导包导入到主程序中，那么当其中一个出现故障的时候，并不会影响到其他的代码。

### - 分级启动

当加载的时候，可以分开加载。如果加载较慢的时候，可以优先加载重要的模块。
---
title: DOM事件模型
date: 2020-06-04 14:14
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

## 一、事件捕获与事件冒泡
事件的捕获和冒泡，分别由微软和网景提出。他们分别决定了事件发生顺序的问题。

```html
<div class="outer">
    <button class="inner">按钮</button>
</div>
```

如果在上面的代码中，outer盒子与inner按钮，分别绑定了一个点击事件。那么他们的执行顺序应该是怎么样呢？

### 1、事件捕获

事件捕获的顺序是由外到内，从最外面的元素开始检测。如果监听到有事件，则开始执行。

执行顺序为 outer ==> inner

### 2、事件冒泡

事件冒泡顺序与事件捕获相反，它是从最里面的元素开始往外检测，检测到有事件的话就开始执行事件。

执行顺序为 inner ==> outer

### 3、W3C规范

在网景和微软的冲突后。W3C开始介入，并将两者合并在一起。

规范为： 先捕获在冒泡

### 4、addEventistener 的第三个参数

```js
element.addEventListener(event, function, useCapture)
```

在监听函数中，第一个为事件，第二个为执行的函数，第三个为是否冒泡

### 5、target和currentTarget的区别

e.target 指向的是被触发的元素

e.currentTarget 指向的是被监听的元素

> this 就是 currentTarget

### 6、取消冒泡

```js
e.stopPropagation()
```

## 二、自定义事件

```js
// 创建自定义事件
element.addEventListener('click', ()=>{
    const event = new CustomEvent('hi', {
        info: {name: "lyw", age: 20},
        bubbles: true, // 是否冒泡
        cancelable: false // 是否可以取消冒泡
    })
    element.dispatchEvent(event)
})

// 调用自定义事件
element.addEventListener('lyw', (e)=>{
    console.log(e.name)
})
```
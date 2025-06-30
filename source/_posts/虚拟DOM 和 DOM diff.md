---
title: 虚拟DOM 和 DOM diff
date: 2020-09-13 23:41
tags:
  - 博客
  - react
  - vue
  - vdom
  - dom-diff
  - 前端
categories:
  - 博客
  - 前端
---

## 一、什么是虚拟DOM

一个能表示DOM树的对象，通常含有标签名、标签上的属性、事件监听和子元素们，以及其他属性

## 二、虚拟DOM有什么优点

### 1. 减少DOM操作

- 虚拟DOM可以将多次操作合并为一次操作。
- 虚拟DOM借助DOM diff 可以将多余的操作省略

### 2. 跨平台
- 虚拟DOM不仅可以变成DOM，还可以变成小程序、iOS应用，因为虚拟DOM本质是JS对象

## 三、虚拟DOM有什么缺点

需要额外的创建函数，如createElement 或 h，但可以通过 JSX 来简化成 XML 语法

## 四、虚拟DOM是什么样子

### React

```js
const vNode = {
  key: null,
  props: {
    children: [
      {type: 'span', ...},
      {type: 'span', ...}
    ],
    className: "red",
    onClick: ()=>{}
  },
  ref: null,
  type: "div",
  ...
}
```

### Vue

```js
const vNode = {
  tag: 'div',
  data: {
    class: 'red',
    on: {
      click: ()=>{}
    }
  },
  children: [
    {tag: "span", ...},
    {tag: "span", ...}
  ],
  ... 
```

## 五、如何创建虚拟DOM

### React

```js
createElement('div', {className: "red", onClick: ()=>{}, [
  createElement('span', {}, 'span1'),
  createElement('span', {}, 'span2')
]})
```

### Vue

```js
h('div', {
  class: 'red',
  on: {
    click: ()=>{}
  }
}, [h('span', {}, 'span1'), h('span', {}, 'span2')])
```

### React JSX

```jsx
<div className="red" onClick="{fn}">
  <span>span1</span>
  <span>span1</span>
</div>
```

### Vue Template

```vue
<div class="red" @click="fn">
  <span>span1</span>
  <span>span1</span>
</div>
```

## 六、什么是DOM diff

DOM diff其实就是一个函数，它会对比 oldNode 与 newNode 的区别。从而减少不必要的渲染。但是DOM diff会有bug。造成页面渲染的误操作，可以使用key来辅助diff的对比。从而消除bug。

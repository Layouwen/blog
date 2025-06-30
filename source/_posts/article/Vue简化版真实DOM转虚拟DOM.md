---
title: Vue简化版真实DOM转虚拟DOM
date: 2021-03-18 11:15
tags:
  - 博客
  - 前端
  - vue
  - vdom
categories:
  - 博客
  - 前端
---

## 1、大致思路

获取#app根节点，创建VNode类用于创建虚拟DOM，创建vDom函数，用于生成虚拟DOM对象。根据元素的 nodeType 判断类型，对真实DOM的参数进行处理创建虚拟DOM。对每个元素的 childNodes 进行遍历，递归的进行 vDom 的创建。

## 2、代码实现

```js
class Vue {
  constructor(option) {
    this.obj = document.querySelector(option.el)
    let AST = vDom(this.obj)
    console.log(AST)
  }
}

class VNode {
  constructor(option) {
    Object.assign(this, option)
    this.children = []
  }
  appendChild(node) {
    this.children.push(node)
  }
}

function vDom(node) {
  let nodeType = node.nodeType
  let _vnode = null
  if (nodeType === 1) { // 元素节点
    let props = node.attributes // 获取元素的属性

    let property = {} // 定义一个空的对象，用于保存精简后的属性
    for (let i = 0; i < props.length; i++) { // 处理属性
      property[props[i].name] = props[i].nodeValue
    }

    // 将处理好的数据，创建虚拟DOM
    _vnode = new VNode({
      tagName: node.nodeName,
      props: property,
      type: nodeType
    })

    let children = node.childNodes // 获取所有子节点
    for (let i = 0; i < children.length; i++) {
      // 遍历循环子节点，只对 元素节点 和 长度大于 1 的节点进行处理
      if (children[i].nodeType === 1 || children[i].length > 1) {
        _vnode.appendChild(vDom(children[i]))
      }
    }
  } else if (nodeType === 3) { // 文本节点
    _vnode = new VNode({
      type: nodeType,
      value: node.nodeValue.trim()
    })
  }
  return _vnode
}

new Vue({
  el: '#app',
  data: {}
})
```

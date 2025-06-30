---
title: 你不知道的WebComponent
date: 2021-04-02 02:39
tags:
  - 博客
  - 前端
  - webcomponent
categories:
  - 博客
  - 前端
---

## 1、原生也有组件？

现在Vue、React的大规模流行。前端组件化已经成为潮流，但是原生的组件又有多少人了解了。下面通过几个代码示例，让你快速了解原生怎么写组件。

## 2、通过继承 HTML 实现组件

[代码demo示例](http://layouwen.gitee.io/webcomponent-demo/demo1.html)

```js
// 通过继承 HTMLImageElement 实现组件化
class MyImg extends HTMLImageElement {
  constructor() {
    super()
    const src = 'https://resource.ttplus.cn/publish/app/pics/2019/04/18/233772/76d96560-7211-483d-988c-dc00d8391f41.jpg'
    setTimeout(() => {
      // this 指向继承后的新组件
      this.src = src
    }, 1000)
  }
}

// 组件名字 继承后的类 需要继承的标签
customElements.define('my-img', MyImg, {
  extends: 'img',
})
```

使用的时候只需要在你继承的标签中，`is="你的组件名"` 即可

```html
<img is="my-img" />
```

## 3、独立的组件

[代码demo示例](http://layouwen.gitee.io/webcomponent-demo/demo2.html)

### demo2.html

```html
<style>
  /* 样式隔离，无法操作 MyCom 内部的样式 */
  .my_com_main {
    background: pink;
  }
</style>

<my-com class="my_com" cusAttr="我是自定义属性">
  <span slot="middle">我是主要内容</span>
  <h2 slot="bottom">我是底部</h2>
  <div slot="head">我是头部插槽</div>
</my-com>
<script type="module" src="./demo2.js"></script>
```

### MyCom.js

```js
const template = document.createElement('template')
template.innerHTML = `
<style>
  .my_com_wrapper {
    border: 1px solid red;
  }
  .my_com_main {
    height: 100px;
    background: skyblue;
  }
</style>
<div class="my_com_wrapper">
  <header>
    <slot name="head"></slot>
  </header>
  <main class="my_com_main">
    <slot name="middle"></slot>
  </main>
  <footer>
    <slot name="bottom"></slot>
  </footer>
</div>
`
class MyCom extends HTMLElement {
  constructor() {
    super()
    // 获取自定义组件的属性
    console.log(this.getAttribute('class'))
    console.log(this.getAttribute('cusAttr'))
    // 使用 attachShadow 与外面样式进行隔离
    const sd = this.attachShadow({ mode: 'open' })
    sd.appendChild(template.content)
  }
}

customElements.define('my-com', MyCom)
```

### demo2.js

```js
import './MyCom.js'

const myComEl = document.querySelector('.my_com')
// 当 attachShadow 的 mode 为 open 时，可以获取 shadowRoot 的内容并且操作。
// 如果 mode 为 closed 则无法获取该节点信息
console.log(myComEl.shadowRoot)
const myColElChildren = myComEl.shadowRoot.children
![...myColElChildren].forEach(el => (el.style = 'font-size: 40px;'))
```

## 4、如何实现自定义事件

[代码demo示例](http://layouwen.gitee.io/webcomponent-demo/demo3.html)

通过 EventTarget 添加自定义事件。在通过 dispatchEvent 触发 CustomEvent 事件。

```html
<button id="btn_a">自定义事件a</button>
<button id="btn_b">自定义事件b</button>
<script type="module">
  // 创建事件
  const obj = new EventTarget()
  // 添加事件
  obj.addEventListener('a', () => alert('触发了事件a'))
  obj.addEventListener('b', () => alert('触发了事件b'))
  document.querySelector('#btn_a').onclick = () => {
    obj.dispatchEvent(new CustomEvent('a'))
  }
  document.querySelector('#btn_b').onclick = () => {
    obj.dispatchEvent(new CustomEvent('b'))
  }
</script>
```

## 5、实现简易版Dialog组件

由于代码太多，直接放链接了

[代码demo示例](http://layouwen.gitee.io/webcomponent-demo/demo4.html)

## 6、仓库源码地址

[https://github.com/Layouwen/webcomponent-demo](https://github.com/Layouwen/webcomponent-demo)
---
title: Vue自定义指令、Mixin、Extends、Provide&Inject
date: 2020-12-21 21:28
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
---

## 一、自定义指令

### 1.1 全局指令

```js
Vue.directive('指令名', directiveOptions)
```

### 1.2 局部指令

```js
new Vue({
  ...,
  directives: {
    '指令名': directiveOptions
  }
})
```

### 1.3 directiveOptions的属性

- bind(el, info, vnode, oldVnode) —— 类似于 created
- inserted(el, info, vnode, oldVnode) —— 类似于 mounted
- update(el, info, vnode, oldVnode) —— 类似于 updated
- componentUpdated(el, info, vnode, oldVnode) —— 基本上用不到
- unbind(el, info, vnode, oldVnode) —— 类似于 destroyed

### 1.4 使用方式

```html
<div v-指令名="参数"></div>
```

### 1.5 代码示例，模拟v-on

```js
new Vue({
  directives: 
    'on2': {
      inserted(el, info){
        el.addEventListener(info.arg, info.value)
      },
      unbind(el, info){
        el.removeEventListener(info.arg, info.value)
      }
    }
  },
  methods: {
    fn(){
      console.log('测试')
    }
  }
})
```

```html
<div v-on2:click="fn">我是假的v-on</div>
```

### 1.6 函数简写

如果 `bind` 和 `update` 的内容一致时，并不关心其他钩子，可以使用此简写。

```js
Vue.directive('color-swatch', (el, binding)=>{
  el.style.backgroundColor = binging.value
})
```

## 二、Mixin混入

需要混入的内容 `mixins/test.js`

```js
data(){
  return {
    msg: '我是测试的内容'
  }
},
methods: {
  onSay(){
    console.log(this.msg)
  }
},
created() {
  console.log('我是测试的生面周期钩子')
}
```

需要混入的文件 `Demo.vue`
```js
import test from '../mixins/test.js'
new Vue({
  mixins: [test]
})
```

> 混入说白了就是复制粘贴

## 三、Extends继承、拓展

需要继承的文件 DemoVue.js

```js
import Vue from 'vue'
import test from '../mixins/test.js'

const DemoVue = Vue.extend({
  mixins: [test]
})
```

需要使用该继承的地方

```js
import DemoVue from '../DemoVue.js'

new Vue({
  extends: DemoVue
})
```

或者

```js
new DemoVue({
  构造选项
})
```

## 四、Provide & Inject

使用 `provide` 将属性暴露出去

Demo1.vue
```js
new Vue({
  data(){
    return {
      color: 'red'
    }
  },
  methods: {
    changeColor(){
      this.color = 'blue'
    }
  },
  project(){
    return {
      color: this.color,
      changeColor: this.changeColor
    }
  }
})
```

其他组件就可以使用 `inject` 获取暴露出来的值
```js
new Vue({
  inject: ['color', 'changeColor'],
  methods: {
    showColor(){
      console.log(this.color)
      this.changeColor()
      console.log(this.color)
    }
  }
})
```
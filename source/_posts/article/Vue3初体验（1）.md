---
title: Vue3初体验（1）
date: 2020-10-05 22:13
tags:
  - 博客
  - vue
  - 前端
categories:
  - 博客
  - 前端
---

## 1、Vite简单操作

### 安装

```bash
yarn global add create-vite-app@1.18.0
```

### 创建项目

文档的命令
```bash
npm init vite-app 项目名
yarn create vite-app 项目名
```

等价于
```bash
全局安装后
cva 项目名
或
npx create-vite-app 项目名
```

## 2、Vue3与Vue2的区别

- Vue3的Template支持多个跟标签，Vue2不支持
- Vue3有createApp()，而Vue2是new Vue()
- createApp(组件)，new Vue({template,render})

## 3、Vue-router 4

### 3.1 查看所有版本号

```bash
npm info vue-router versions
```

### 3.2 安装vue-router 4

```bash
yarn add vue-router@4.0.0-beta.3
```

### 3.3 初始化vue-router

#### 新建history对象

在main.ts中添加

```js
import {createWebHashHistory, createRouter} from 'vue-router'

const history = createWebHashHistory()

```

#### 新建router对象

```js
const router = createRouter({
  history,
  routes: [
    {path: '/', component: Easyw},
  ],
});

```

#### app.use(router)

```js
const app = createApp(App);
app.use(router);
app.mount('#app'); // 挂载组件
```

#### 在App.vue中添加router-view

```html
<template>
  <router-view/>
</template>
```

#### 在routers中添加其他测试路由

```js
routers: [{
    {path: '/', component: Easyw},
    {path: '/test', component: Test},
}]
```

#### 在App.vue中添加router-link

```html
  <router-link to="/">Easyw</router-link>
  <router-link to="/test">Test</router-link>
```

### 3.4 添加子路由

```js
const history = createWebHashHistory();
const router = createRouter({
  history,
  routes: [
    {path: '/', component: Home},
    {
      path: '/doc', component: Doc, children: [
        {path: 'switch', component: SwitchDemo},
        {path: 'switch', component: SwitchDemo},
      ],
    },
  ],
});
```

### 3.5 路由切换时进行操作

先导入你的router文件
```js
import router from './router'
```

```js
router.afterEach(() => {
  console.log(1);
});
```

## 4、provide和inject

### 4.1 实现思路

在最外层的页面中，定义provide变量。然后在子组件中使用inject可以及时拿到该变量。

 ## 4.2 使用步骤
 
 在最外层声明
 App.vue
 ```html
<script lang="ts">
// 导入相应内容
import {provide, ref} from 'vue

export default {
  setup(){
    // 使用ref设置默认值
    const asideVisible = ref(false)
    // 使用provide设置名字以及它对应的值，供子组件访问其数值
    provide('asideVisible', asideVisible)
  }
}
</script>
```

子组件.vue
```html
<script lang="ts">
  // 导入相应内容
  import {inject, Ref} from 'vue';

  export default {
    setup() {
      // 使用inject获取名为asideVisible的provide
      const asideVisible = inject<Ref<boolean>>('asideVisible');
    },
  };
</script>
```

## 5、props外部传参

### 5.1 实现思路

在外部定义一个需要传参的属性名，后面带上需要传递的参数。在子组件中使用props接受该参数。如果需要修改使用context.$emit和$event进行数据的修改。

### 5.2 使用步骤

#### 外部组件

定义属性

```js
setup(){
  const value = ref(true)
}
```

传递参数，并定义事件名

```html
    <Switch :value="value" @update:value="value = $event"/>
```

#### 子组件

接受参数

```js
props: {
  value: Boolean
}
```

修改参数

```js
setup(props, context){
  const modify = () => {
    context.$emit('update:value', !props.value)
  }
}
```

### 5.3 使用v-model简化

删除外部事件名，使用v-model代替

```html
    <Switch v-model:value="value"/>
```

> 注意：如果使用v-model，子组件内部的触发事件名必须为 `update:外部定义的参数名`
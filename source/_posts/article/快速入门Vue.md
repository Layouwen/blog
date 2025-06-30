---
title: 快速入门Vue
date: 2020-07-15 02:03
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
---

## Vue实例的options属性


### 一、数据


#### data 内部数据


```vue
// 对象形式
data: {
    n: 0
}

或

// 函数形式
data() {
    return {
        n: 0
    }
}
```



> 优先使用函数，因为如果调用多次，会出现对象被多次引用的问题
>
> 使用数据可以使用 this.数据名
>



#### props 外部数据


接收外部数据



```vue
<template>
    <div>
    {{外部数据名}}
    </div>
</template>

<script>
export default {
    porps: ['外部数据名']
}
</script>
```



传外部数据



```vue
<组件名 外部数据名="数据"/>
```



> 如果你传的是变量或方法，需要加上冒号
>



```vue
<组件名 :外部数据名="变量名或方法名"/>
```



#### propsData


#### computed 计算属性


#### methods 方法


```vue
new Vue({
    method: {
        函数名(){
            函数体
        }
    }
})
```



#### watch 监听


### 二、DOM


#### el 挂载点


```vue
new Vue({
    el: '#app'
})
```



执行后会将#app标签整个替换掉



> 还可以使用 new Vue({}).$mount(#app) 来挂载
>



#### template HTML


#### render 非完整版的HTML


#### renderError 用不到


### 三、生命周期


beforeCreate 创建前



created 创建后



beforeMount 挂载前



mounted 挂载后



beforeUpdate 更新前



updated 更新后



activated



deactivated



beforeDestroy 销毁前



destroyed 销毁后



errorCaptured 用不到



```vue
new Vue({
   beforeCreate(){
    console.log('创建前执行')
   },
   created(){
    console.log('创建后执行')
   },
   beforeMount(){
    console.log('挂载到页面前执行')
   },
   mount(){
    console.log('挂载到页面后执行')
   }，
   beforeUpdate(){
    console.log('数据更新前执行')
   },
   update(){
    console.log('数据更新后执行')
   },
   beforeDestroy(){
    console.log('消亡前执行')
   },
   destroyed(){
    console.log('消亡后执行')
   },
})
```



### 四、资源


#### directives 指令


#### filters 过滤器


#### components 组件


方法1



```vue
Vue.component('组件名',{与Vue实例的属性一致})
```



方法2



创建单文件组件，以 `.vue` 结尾的文件，在Vue实例中引入



```vue
import Demo from './Demo'

new Vue({
    components: {Demo}
})
```



然后即可直接在HTML中使用



```html
<Demo/>
```



> 推荐优先使用方法2
>



### 五、组合


#### parent 父


#### mixins 混入


#### extends 拓展


#### provide 提供


#### inject 注入

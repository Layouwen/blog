---
title: Vue 篇
date: 2021-03-22T23:48:34.000Z
tags:
  - 汇总
  - 知识点总结
  - vue
categories:
  - 汇总
  - 知识点总结
uuid: 8a59ab73-6116-4644-b6ab-8e9f3e03e802
---
# Vue2

## 前言

这里是我总结一些与Vue2.x相关的一些面试题。本文章基于2.x进行总结。在3.0中可能会有不同。

## 1、v-if和v-for优先级？同时使用怎么优化？

vue2

- 根据测试，同时使用v-if和v-for时，v-for优先级更高。
- 同时使用v-for会必然执行，为了不浪费性能，可以在外面嵌套template使用v-if，内部使用v-for。
- 如果条件在v-for内部，可以通过计算属性过滤掉不需要显示的内容。

vue3

- v-if 比 v-for 优先

## 2、组件中的data为什么必须是函数？


- 组件可能存在多个实例，使用对象形式则会共用一个data对象，从而造成污染。
- 使用函数形式，在源码中 initState 中的 initData 方法过程中会将data的函数执行一遍获取函数的返回值，再将函数的返回值作为组件的数据，避免状态污染。
- 之所以Vue根实例没有限制，是因为正常每个项目只有一个根实例，我们不会 new 多个Vue出来，所以不担心被污染。在源码中，mergeOption 时，我们如果是为根实例会传第三个此参数也就是 vm 的引用，此时我跳过了 data 的判断，在 comp 中我们是不会传vm参数，则进入到 data 类型的判断。

## 3、key的作用和原理？

updateChildren 中通过 patchVnode函数，他会对比新旧两个节点是否一致从而进行一个更新。如果不传key的时候，默认值为undefined，因为undefined与undefined比较为ture，则默认这个相同的节点，从而不进行更新。因此会触发很多bug。

- key的作用使用来提高虚拟DOM效率。
- key在DOMdiff中可以判断两个节点是不是一个节点，避免不必要的更新。
- 如果不设置key会有潜在的bug。比如：没更新到input的value值，更新了不该更新的内容。
- 具体比较为首尾对比

## 4、什么是diff算法

- diff算法是虚拟DOM的必然产物，通过与旧的虚拟DOM做比较，将变化的更新在真实DOM中。
- 在 Vue2.x 中，一个实例对应一个 Watcher 所以无法像 Vue1中一样精确的更新对应的Dom元素。当数据发生改变时，数据响应式会触发 set，set会触发update，在update中，通过观察者模式，会触发对应的Watcher实例。此时我们的Watcher会进行render函数的渲染，所以我们会触发diff算法，找出发生改变的节点，进行局部更新。
- diff执行时，它将新的虚拟DOM与之前旧的虚拟DOM做比较（patch）。
- diff比较时，会判断本身和首尾子节点是否一样，如果没有执行遍历对比直至处理剩下的所有节点。使用key可以对这个过程进行加快。

概括
- 同层级 diff, 不跨层 diff
- 双端比较(头尾指针)
	- 旧头新头
	- 旧尾新尾
	- 旧头新尾
	- 旧尾新头

减少 DOM 移动, key 用于提高复用准确率
## 5、vue组件化

- 组件有独立和复用性作用，可以大幅提高开发效率。
- 组件使用按分类有：页面组件、业务组件、通用组件，合理的划分组件，有助于提升应用性能。
- 常见的组件化技术有：属性prop，自定义事件，插槽等，它们主要用于组件通信、扩展等。

## 6、vue设计原理

- 它是一个渐进式js框架。它自底向上逐层应用。
- Vue核心库只关心视图层，易于上手并便于与第三方库整合。

## 7、MVC、MVP和MVVM的理解？

- 这些都是框架模式，为了解决Model和View的耦合度。实现model层和view层分层管理。
- MVC最早应用在后端。优点是分层清晰，缺点是数据流混乱。Model扶着保存应用数据，与后端进行同步。Controller负责业务逻辑，根据行为对Model的数据进行修改。View负责视图展示，装model的数据进行可视化。
- MVP是MVC的升级版。Presenter作为中间层负责MV通信。但负责内容太多，导致臃肿难以维护。
- MVVM在前端广泛应用。将两者优点相结合。

## 8、性能优化？

- ssr 可以改善首屏渲染, 但是考验服务端的性能

- 不要在生产环境区编译模板

- data 层级不要太深, 因为是一次性进行递归监听

- 合理使用 computed 缓存数据

- 路由懒加载
```js
const router = new VueRouter({
  routes: [
 	 { path: '/foo', component: () => import('./Foo.vue') }
  ]
})
```

- keep-alive缓存页面
```vue
<template>
  <div id="app">
    <keep-alive>
      <router-view/>
    </keep-alive>
  </div>
</template>
```

- v-show复用DOM
```vue
<template>
  <div class="cell">
    <!--这种情况用v-show复用DOM，比v-if效果好-->
    <div v-show="value" class="on">
      <Heavy :n="10000"/>
    </div>
    <section v-show="!value" class="off">
      <Heavy :n="10000"/>
    </section>
  </div>
</template>
```

- v-for避免使用v-if
```vue
<template>
  <ul>
    <li v-for="user in activeUsers" :key="user.id">
      {{ user.name }}
    </li>
  </ul>
</template>
<script>
export default {
  computed: {
    activeUsers: function () {
      return this.users.filter(function (user) {
        return user.isActive
      })
    }
  }
}
</script>
```

- 长列表性能优化
1. 如果列表是纯粹的数据展示，不会有任何变化，就不需要做响应化
```js
export default {
  data: () => ({
    users: []
  }),
  async created() {
    const users = await axios.get("/api/users");
    this.users = Object.freeze(users);
  }
};
```
2. 如果是大数据长列表，可采用虚拟滚动，只渲染少部分区域的内容
```vue
<recycle-scroller class="items" :items="items" :item-size="24">
  <template v-slot="{ item }">
    <FetchItemView:item="item" @vote="voteItem(item)"/>
  </template>
</recycle-scroller>
```

- 事件销毁
```js
created() {
  this.timer = setInterval(this.refresh, 2000)
},
beforeDestroy() {
  clearInterval(this.timer)
}
```

- 图片懒加载
```vue
<img v-lazy="/static/img/1.png">
```

- 第三方库按需引入
    像element-ui这样的第三方组件库可以按需引入避免体积太大。

- 如果组件是一个无状态的组件，只是接收数据并渲染出来，可以设置为函数组件

```html
<template functional>
  // ...
</template>
```
    
## 9、组件通信的方式

- 三种场景：父子、兄弟、跨组件
1. props（父子传值）
2. EventBus或\$emit/$on（全局或父子通信）
	1. vue3 后基本用 mitt 构建
3. ref 与 $parent / $children（父子组件通信）
4. vuex（全局）
    - state：存放数据
    - getter：对数据进行处理，并返回数据
    - mutations：更改state的数据。接收state作为第一个参数，payload为第二个参数。
    - action：调用mutaltion对数据进行处理，它可以进行异步操作。触发方式：store.dispatch，mutation通过store.commit提交
5. \$attrs/$listeners（隔代组件通信）
    - $attrs：包含所有父组件未使用props标注的值
    - $listeners：包含所有父组件v-on的事件监听器
6. provide/inject（隔代组件通信）

## 10、有没有使用过Vuex？

- 它是vue的全局状态管理（登录验证、购物车、播放器）
- 它是单项数据流，用State存放数据，通过Mutation修改数据。如果需要用到异步可以使用Action。但是Action无法直接修改数据，需要调用Mutation来修改State的数据
- 虽然Vuex的数据是响应式的，但是并不会保存起来。需要使用Storage进行保存。在读取出来替换State的数据

## 11、VueRouter有什么钩子？

1. 全局钩子
    - beforeEach(to, from, next)——路由改变前调用
        - 常用验证用户权限
        - 参数to：即将进入目标路由的对象
            - from：正要离开的路由对象next：路由控制参数
            - next()：一切正常，则进入下一个钩子
            - next(false)：取消导航（不改变）
            - next('/login')：中断导航，进入新的导航
            - next(error)：终止导航，传递参数给 router.onError()
    - afterEach(to, from)——路由改变后调用
        - 常用返回页面顶端
        - 用法类似，但少了next参数
2. 路由配置中的钩子
    - beforeEnter(to, from, next)
3. 组件内的钩子
    - beforeRouteEnter(to, from, next)
        - 组件被comfirm前调用
        - 比vue实例化快，所以无法使用this
    - beforeRouteUpdate(to, from, next)
        - 路由改变后，组件被复用时调用
        - 在vue实例化后，可以使用this
    - beforeRouteLeave(to, from, next)
        - 离开组件对应路由时调用
        - 可以访问this
4. 路由监测变化
    - 监听到路由变化，对齐进行操作
    - 路由钩子：全局、组件、路由配置
    - 使用watch对$route监听

## 12、递归组件

问的不多，我当时没答上来。可以了解一下Vue实现的收缩框和多级菜单

## 13、Vue的响应式原理

- 将data使用观察者模式Observer对数据转换成getter和setter进行监听
    - 监听数据变化
    - 收集依赖到的数据
    - 当他变化时，通知视图层进行更新

## 14、怎么拓展现有组件

- 使用mixin全局混入
- 使用slot拓展

## 15、watch和computed了解多少

- watch监听属性，computed计算属性
- watch更为通用，计算属性有缓存功能
- 有限使用computed，如果需要监听数值变化后与后端交互使用watch
- watch：执行异步或开销较大的操作时使用。如：搜索功能
- computed：复杂逻辑或一个属性影响多个属性变化时执行。如：购物车结算

## 16、有没有了解过nextTick？

- 在DOM更新完毕后执行回调，获取更新后的DOM
- Vue的DOM更新是异步的，只要监听到数据变化，就会开启一个队列。缓冲同一时间循环中的数据变更。如果同一个watcher被多次触发，他会进行一个去重，避免不必要的DOM操作。nextTick会在队列中添加一个回调，在前面的DOM操作结束后调用


```js
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function(){
  console.log('DOM 更新了')
})
```

## 17、Vue的生命周期

没什么好说的。单词翻译过来就知道了，按顺序排列

- beforeCreate：this不能使用，data和methods以及wather的事件还不能获取
- created：可以操作vue的数据和方法，还不能操作dom
- beforeMount：render函数首次被调用
- mounted：可以操作dom
- beforeUdata：data数据更新完毕，页面视图没有响应更改
- updated：数据和视图更新完毕
- beforeDestory：在销毁之前
- destoryed：数据、指令全部销毁

## 18、DOM在哪个周期完成，什么时候可以操作DOM？

在Munted时已经渲染完成，渲染完成后即可操作DOM节点

## 19、单向绑定和双向绑定的demo

单向绑定

```vue
<template> 
  <div>{{value}}</div>
</template>
<script>
new Vue(
  data(){
    return {
      value: 10    
    }
  }
)
</script>
```

双向绑定

```vue
<template>  
  <div>    
    <input type="text" v-model="value"/>    
    <div>{{value}}</div>  
  </div>
</template>
<script>
new Vue(
  return {
    value: ''  
  }
)
</script>
```

## 20、v-model触发了哪些语法糖

```vue
<input :value="test" @input="test = $event.target.value" />
```

> 触发了v-bind和v-on的语法糖

## 21、router和route的区别

### 21.1 区别

`$router` 访问路由实例
`$route` 获取当前激活路由的状态信息

> `$router` 为一个容器，管理一组 `$route`

### 21.2 $router对象属性

- $router.app：配置了 `router` 的 `Vue` 跟根实例
- $router.mode：路由使用的模式
- $router.currentRoute：当前路由对应的路由信息对象

### 21.3 $route对象属性

- $route.path：返回当前路由的路径
- $route.params：返回路由参数 `key-value` 对象，如果没有路由参数，返回空对象
- $route.query：返回查询参数 `key-value` 对象
- $route.hash：返回路由中带#的hash值，没有则为空字符串
- $route.fullPath：返回解析后的 `url` ，包含了查询参数和当前路由名称
- $route.name：返回当前路由名称
- $route.redirectedFrom：如果存在重定向，则返回重定向之前的名字

## 22、router的三种模式

### 22.1 hash模式

使用url中的hash模拟一个完整的url，页面改变的时候，页面不会重新加载

### 22.2 history模式

利用history.pushState的api完成url跳转，但是这个方法需要后端进行相应的配置，不然访问一些路由以外的路径会导致404

### 22.3 abstract模式

如果检测到没有浏览器api，会自动进入这个模式

## 23、vue-router和location.href

- vue-router使用pushState进行路由更新，静态跳转，页面不会重新加载。
- location.href触发浏览器跳转，页面会重新加载一次
- vue-router是路由跳转或者同页面跳转
- location.href是不同页面的跳转
- vue-router是异步加载，可以使用this.$nextTick(()=>{获取url})
- location.href是同步加载

## 24、new Vue做了什么

1. 创建了Vue实例，内部执行了根实例的初始化过程
2. 具体包括以下
    1. 选项合并
    2. $children/$refs/$slots/$createElement等实例属性和方法的初始化
    3. 自定义事件的处理
    4. 数据响应式处理
    5. 生命周期钩子调用
    6. 可能的挂载
3. 总结：new Vue()创建了根实例并准备好数据和方法，未来执行挂载时，此过程还会递归的应用它的子组件上，最终形成一个有紧密关系的组件示例树

## 25、如何把真实DOM转变虚拟DOM

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

## 26、vm.$set()为什么可以解决新增属性不能响应的问题

1. Vue使用Object.defineProperty实现双向数据绑定
2. 在初始化示例时对属性进行 getter/setter 转化
3. 属性必须在data对象上存在才能让Vue转化响应式（导致新增对象的属性Vue无法检测到）

> 所以Vue提供了 Vue.set(Object, propertyName, value) / vm.$set(object, propertyName, value)

## 27、Template的实现思路

1. 将模板字符串转化为 element ASTs（解析器）（abstract syntax tree，抽象语法树）
2. 对 AST 进行静态节点标记，主要用于虚拟DOM的渲染优化（优化器）
3. 使用 element ASTs 生成 render 函数代码字符串（代码生成器）

## 28、Vue初始化流程

```shell
git clone git@github.com:vuejs/vue.git
```

## 29、对Vue3新特性有没有了解？

- 更快
    - 虚拟DOM重写
    - 优化slots的生成
    - 静态树提升
    - 静态属性提升
    - 基于Proxy的响应式系统
- 更小：通过摇树优化核心库的体积
- 更好的维护：TypeScript + 模块化
- 更加友好：
    - 跨平台：编译器核心和运行时核心与平台无关
- 更容易使用
    - 提供强有力的类型检查和错误警告
    - 独立的响应式模块
    - Comosition API

# Vue3

## vue3 比 vue2 有哪些优势

- 性能更好
- 更好的逻辑组织和抽离
	- composition
- 对 ts 更好的支持
	- vue2 需要额外的库和额外的写法去编写
- 体积更小

## Vue3 生命周期的区别和改动

- option api
	- beforeDestroy 改为了 beforeUnmount
	- destroyed 改为 unmounted
	- 其他沿用原本
- composition api
	- 声明周期
		- setup 等价于 beforeCreate 和 created
		- onBeforeMount
		- onMounted
		- onBeforeUpdate
		- onUpdated
		- onBeforeUnmount
		- onUnmounted
		- onActivated
		- onDeactivated
		- onErrorCaptured
		- onRenderTracked
		- onRenderTriggered
		- onServerPrefetch
	- 带来了什么
		- 更好的代码组织和逻辑复用
		- 更好的类型推导

## 如何理解 ref toRef toRefs

- ref 声明一个响应式值
- toRef 将 reactive 中的属性变成响应式 ref 值
- toRefs 因为 reactive 声明的响应式对象, 如果进行解构里面的属性将失去响应式, 所以需要 toRefs 进行包裹, 使其属性变成响应式值
- toRefs 也常用于 composition 返回值通过解构的时候取值
- toRef 和 toRefs 都是为了延续响应式

## Proxy 和 defineProperty 区别

- proxy 无法兼容所有浏览器, 无法 ployfill
- proxy 无需想 defineProperty 一样做新增属性和删除属性的额外处理, 以及对数组 api 的额外处理
- proxy 不需要一次性递归监听所有属性

## v-model 的改变

父组件对属性使用 `v-model` 的时候传值的同时, 传递一个 `update:值` 的事件用于更新值, 实现双向绑定
子组件可以通过触发 `emit('update:值')` 来更新传递的值

## reactive 和 ref 的区别和使用场景

## watch 和 watchEffect 区别

- 他们都是用于监听 data 属性的变化
- watch 需要明确监听哪个属性, 可以通过 immediate true 立马触发监听执行, 以及通过 deep true 进行深度监听
- watchEffect 会自动监听依赖的数据变化, 初始化就会直接执行一次

## setup 如何获取组件实例

通过 `getCurrentInstance` 获取组件实例

## 为什么 vue3 比 vue2 快

- 使用 proxy 响应式
- PatchFlag
	- 编译模板的时候, 为不同节点做标记
	- 通过分析模板代码, 在 createVNode 的第四个参数, 会给一个数子以及一个注释表示哪一种标记, 如果含有 props, 会在第五个参数表示用到了哪些 props
	- 为 diff 算法的时候, 可以区分不同节点, 并跳过静态节点
- hoistSatic
	- 将静态节点定义, 提升到父作用域缓存, 使用空间换时间策略
	- 多个相邻静态节点, 合并起来. 在编译的时候做优化, 参考很多编译器提前算好结果也一样.
- cacheHandler
	- 将事件函数缓存起来, 不需要每次都创建一个函数
- SSR 优化
	- 静态节点直接输出编译好的内容, 在拼接动态内容
- tree-shaking
	- 模板编译的时候, 按使用情况引入相关的库

Proxy 响应式**
避免：
- 深层递归
- 数组 hack
- 属性新增问题

编译时优化（最核心)
包括：
**PatchFlag**
只更新动态节点。
**静态提升（hoistStatic）**
静态节点只创建一次。
**事件缓存（cacheHandler）**
避免重复创建函数。
**BlockTree**
只跟踪动态节点树。
**更好的 diff**
静态节点跳过比较。  
**Tree-Shaking**
按需引入减少体积。
## Vite 为什么快

- 开发环境启动快, 使用 ES6 Module 不需要打包

## Composition API 和 React Hooks 对比

- 前者 setup 只执行一次, 后者每次都会调用执行
- 前者无需关心 useMemo useCallback, 后者每次执行需要考虑缓存问题
- 前者无需关心顺序, 后则组要保证顺序
- reactive + ref 的概念比 useState 要繁琐一点

## vue3 响应式核心

TODO: 补充完整

- reactive 对象代理
- ref 值代理
- track 依赖收集
- trigger 派发更新
- effect 副作用函数

**Proxy 优势**
可以直接拦截：
- get
- set
- deleteProperty
- has
- ownKeys

无需深度初始化。

## Vue 主线

**1. 响应式**

Vue2：
- defineProperty
- Dep
- Watcher

Vue3：
- Proxy
- effect
- track/trigger

**2. 编译**

template：
→ AST
→ render
→ vnode

**3. 渲染**

- patch
- diff
- patchFlag
- blockTree


**4. 组件化**

- props
- emit
- provide
- slot

**5. 工程化**

- vite
- tree-shaking
- 按需加载

**6. 性能**

- keepAlive
- 虚拟列表
- SSR
- 懒加载

# vue2 和 vue3 时期 vue-router 和 vuex/pinia 的区别

vue2: 生态库通过 new Vue 借用响应式
vue3: 生态库直接使用 reactive/ref/effect 构建响应式系统
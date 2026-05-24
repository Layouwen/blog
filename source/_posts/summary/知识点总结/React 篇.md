---
title: React 篇
date: 2021-03-22T18:11:34.000Z
tags:
  - 汇总
  - 知识点总结
  - react
categories:
  - 汇总
  - 知识点总结
uuid: 2d8c4802-ca3a-42dd-96b0-358a5638c733
---
## 1、组件通信方式

1. 父传子：通过Props属性进行接收
2. 子传父：父级传送一个函数，来修改父组件的数据
3. 跨组件通信：通过使用context API，可以在组件内部向子孙级组件通信
context.js
```js
import { createContext } from 'react'
const context = createContext(undefined)
const { Provider, Consumer } = context
export { Provider, Consumer }
export default context
```
父亲组件
```jsx
import{ Provider } from './context'
import { useState } from 'react'

const Father = () => {
const [myName, setMyName] = useState('layouwen')
  return <>
    <Provider value={{myName}}>
      <Son />
    </Provider>
  </>
}
```
子组件
```jsx
import { Consumer } from '../context'

const Son = () => {
  return <></>
}
```

## 2、如何实现路由懒加载

React16新增lazy方法实现组件懒加载。

```jsx
import { Router } from "react-router-dom"
import React, { Suspense } from 'react'
const HomeView = React.lazy(()=>import('./home'))
function App(){
  return <div>
    <h1>路由懒加载</h1>
    <Route path="/" exact render={()=>{
      return <Suspense fallback={<div>组件Loading进来之前的占位内容</div>}
    }}>
      <HomeView />
    </Suspense>
  </div>
}
export default App
```

## 3、生命周期有哪些

- constructor：初始化组件state等
- static getDerivedStateFromProps()：用于将props的信息映射到state中
- render：生成虚拟DOM
- componentDidMount：通常在这里进行副作用的处理（更新阶段）
- static getDerivedStateFromProps()
- shouldComponentUpdate()：判断是否进行组件更新
- render()：生成虚拟DOM
- getSnapshotBeforeUpdate()：完成diff，即将更新虚拟DOM，用户获取上一次DOM快照。PS：必须配合componentDidUpdate一起使用，返回值变成componentDidUpdate 第三个参数
- componentDidUpdate()：组件更新完成，进行副作用处理
- componentWillUnmount：组件即将卸载，删除组件添加的全局数据或事件

![Image _2_.png](https://i.loli.net/2021/09/08/sEtVrwFXnyS2xaJ.png)

## 4、Hooks注意的问题和原因？

### 4.1 注意事项
1. 只能在函数组件使用
2. 只能在外层调用，不能再if、for语句中
3. useState是引用类型的数据，修改state时，要返回新的引用

### 4.2 原因
1. hooks为函数组件逻辑复用而设计
2. hooks调用是要确保调用顺序，顺序错误会导致整个程序的混乱
3. useState更新时不更新引用地址，会导致React认为没有更新数据，则不进行组件更新。

## 5、setState是异步还是同步？

在React的方法中或者事件中是异步的。
在原生事件或异步程序中是同步的。
可以了解一下batchedUpdates

## 6、如何实现逻辑复用

1. HOC（高阶组件）
一个函数接收一个组件，返回一个组件

Acmp本身不具有路由信息，通过withRouter加工使其获得路由信息

```js
function Acmp(props){
  const { history, location, match } = props
  return <div>视图</div>
}

const Bcmp = withRouter(Acmp)
```

withRouter大致实现

```js
const withRouter = (Cmp) => {
  return () => {
    return <Route component={Cmp} />
  }
}
```

2. render props
组件的一个属性，接收一个组件
3. hooks
因为高阶组件返回的是一个集合的信息。hooks可以返回某个功能的信息。

```js
function Acmp(props){
  const { history, location, match } = props
  return <div>视图</div>
}

const Bcmp = withRouter(Acmp)
```

## Class 组件中函数调用的 this 问题

- 在构造函数中提前 bind this, 后续直接调用
- 在声明函数的时候, 使用箭头函数的方式
- 在每次传参调用的时候, 手动 bind this

## React 事件

- react 的事件是属于合成事件, 模拟了原生事件的属性
- 在 17 之前, 绑定在 document 上面, 17 开始绑定在 root 组件上, 有利于多个版本的 react 存在, 如微前端

## React 和 vue 的区别

- 都支持组件化
- 都是数据驱动
- 都使用 vdom
- react 偏向 jsx 使用, vue 偏模板
- react 是函数式, vue 是声明式
- react 把大部分性能优化交给自己, vue 内部做了大部分优化

# Webpack

loader 从后往前的加载顺序

图片资源
dev 模式直接 file-loader 引用
prod 模式使用 url-loader

打包的文件带上 hash 值

需要分离 css 文件, extract css
压缩文件 terser optimizeCSS

分割代码, 抽离公共代码和大的第三方模块采用 cdn 加载

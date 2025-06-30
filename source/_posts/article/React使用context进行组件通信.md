---
title: React使用context进行组件通信
date: 2021-07-07 10:51
tags:
  - 博客
  - 前端
  - react
categories:
  - 博客
  - 前端
---

## 1、介绍

之前文章介绍了使用Props进行参数和函数引用的传参达到组件通信的效果。这次使用context来实现这个效果。

## 2、创建组件

创建 `Grandpa`、`Father`、`Son1`、`Son2`、`Son3` 组件

```jsx
const Grandpa = () => {
  return (
    <>
      <div>我是 Grandpa</div>
      <Father />
    </>
  )
}

const Father = () => {
  return (
    <>
      <div>我是 Father</div>
      <Son1 />
      <Son2 />
    </>
  )
}

const Son1 = () => {
  return (
    <>
      <div>我是Son1</div>
    </>
  )
}

class Son2 extends React.Component {
  render(){
    return <div>我是 Son2</div>
  }
}

const Son3 = () => {
  return (
    <>
      <div>我是 Son3</div>
    </>
  )
}

function App() {
  return (
      <div className="App">
        <Grandpa />
      </div>
  )
}
```

## 3、创建 context 对象

引入 `createContext` 函数，用于构造context对象

```jsx
import { createContext } from 'react'
const context = createContext()
```

从 `context` 中结构出 `Provider`、`Consumer` 两个组件

```jsx
const { Provider, Consumer } = context
```

## 4、注入数据

在 App 中使用 Provider 组件进行注入，并把对应数据和方法传递下去

```jsx
function App() {
  const [name, setName] = useState('梁又文')
  const data = {
    name,
    age: '20',
    setName,
  }
  return (
    <Provider value={data}>
      <div className="App">
        <Grandpa />
      </div>
    </Provider>
  )
}
```

## 5、子孙组件获取数据

### 5.1 函数组件

通过 `Consumer` 获取数据

Son1
```jsx
const Son1 = () => {
  return (
    <Consumer>
      {({ name, setName }) => (
        <div>
          我是Son1，我拿到的数据是：{name} <button onClick={() => setName('我是被Son1修改的名字')}>修改名字</button>
        </div>
      )}
    </Consumer>
  )
}
```

### 5.2 类组件

通过 `contextType` 挂载 `context` 的属性

Son2
```jsx
class Son2 extends Component {
  static contextType = context
  render() {
    const { name, setName } = this.context
    return (
      <>
        我是Son2，我拿到的数据是：{name} <button onClick={() => setName('我是被Son2修改的名字')}>修改名字</button>
      </>
    )
  }
}
```

### 5.3 useContext 方式

通过把 `context` 对象传入到 `useContext` 中，返回所有数据。

```jsx
const Son3 = () => {
  const { name, setName, age } = useContext(context)
  return (
    <div>
      我是Son1，Grandpa今年{age}岁了。我拿到的数据是：{name}
      <button onClick={() => setName('我是被Son1修改的名字')}>修改名字</button>
    </div>
  )
}
```

## 6、最终代码

```jsx
import { Component, createContext, useState, useContext } from 'react'
const context = createContext()
const { Provider, Consumer } = context
export { Consumer, context }

const Grandpa = () => {
  return (
    <>
      <div>我是 Grandpa</div>
      <Father />
    </>
  )
}

const Father = () => {
  return (
    <>
      <div>我是 Father</div>
      <Son1 />
      <Son2 />
      <Son3 />
    </>
  )
}

const Son1 = () => {
  return (
    <Consumer>
      {({ name, setName }) => (
        <div>
          我是Son1，我拿到的数据是：{name} <button onClick={() => setName('我是被Son1修改的名字')}>修改名字</button>
        </div>
      )}
    </Consumer>
  )
}

class Son2 extends Component {
  static contextType = context
  render() {
    const { name, setName } = this.context
    return (
      <>
        我是Son2，我拿到的数据是：{name} <button onClick={() => setName('我是被Son2修改的名字')}>修改名字</button>
      </>
    )
  }
}

const Son3 = () => {
  const { name, setName, age } = useContext(context)
  return (
    <div>
      我是Son1，Grandpa今年{age}岁了。我拿到的数据是：{name}
      <button onClick={() => setName('我是被Son1修改的名字')}>修改名字</button>
    </div>
  )
}

function App() {
  const [name, setName] = useState('梁又文')
  const data = {
    name,
    age: '20',
    setName,
  }
  return (
    <Provider value={data}>
      <div className="App">
        <Grandpa />
      </div>
    </Provider>
  )
}

export default App
```
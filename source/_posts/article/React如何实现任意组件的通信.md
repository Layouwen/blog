---
title: React如何实现任意组件的通信
date: 2020-08-30 11:46
tags:
  - 博客
  - 前端
  - react
categories:
  - 博客
  - 前端
---

## 场景

现在有一个家庭，他们有10w的财产。家庭成员分别有爸爸1、爸爸2。他们分别有两个孩子，儿子11、儿子12、儿子21、儿子22。现在要实现家族中每位成员花费金额时，其他成员都需要知道。

## 方法一 eventHub（非单项数据流）

找个跑腿的。在每次消费后去通知另一位成员。

### 代码

#### HTML

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="https://cdn.bootcdn.net/ajax/libs/react/17.0.0-rc.0/umd/react.production.min.js"></script>
  <script
    src="https://cdn.bootcdn.net/ajax/libs/react-dom/0.0.0-0c756fb-f7f79fd/umd/react-dom.production.min.js"></script>
</head>
<body>
<div id="root"></div>
</body>
</html>
```

#### CSS

```css
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.home {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  color: #fff;
  background: #000;
}

.father {
  padding: 10px;
  margin-top: 10px;
  border: 1px solid #fff;
}

.son {
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #fff;
}
```

#### JSX

```jsx
// 财产
let money = {
  amount: 100000
}

// 跑腿需要监听的事件
let fnLists = {}
let eventHub = {
  trigger(eventName, data) {
    let fnList = fnLists[eventName]
    if (!fnList) return
    for (let i = 0; i < fnList.length; i++) {
      fnList[i](data)
    }
  },
  on(eventName, fn) {
    if (!fnLists[eventName]) {
      fnLists[eventName] = []
    }
    fnLists[eventName].push(fn)
  }
}

class App extends React.Component {
  constructor() {
    super()
    this.state = {
      money: money
    }
  }

  render() {
    return (
      <div className="home">
        <Father1 money={this.state.money}/>
        <Father2 money={this.state.money}/>
      </div>
    )
  }
}

class Father1 extends React.Component {
  constructor() {
    super()
    this.state = {
      money: money
    }
  }

  render() {
    return (
      <div className="father">
        Father1 {this.state.money.amount}
        <Son11 money={this.state.money}/>
        <Son12 money={this.state.money}/>
      </div>
    )
  }
}

class Son11 extends React.Component {
  constructor() {
    super()
    this.state = {
      money: money
    }
  }

  render() {
    return (
      <div className="son">
        son11 {this.state.money.amount}
      </div>
    )
  }
}

class Son12 extends React.Component {
  constructor() {
    super()
    this.state = {
      money: money
    }
    // 监听别人是否花钱
    eventHub.on("我想花钱", (data)=>{
      this.setState({
        money: money
      })
    })
  }
  // 通知跑腿需要花钱
  x() {
    money.amount -= 100
    eventHub.trigger("我想花钱", 100)
    this.setState({
      money: money
    })
  }

  render() {
    return (
      <div className="son">
        son12 {this.state.money.amount}
        <button onClick={() => this.x()}>花钱</button>
      </div>
    )
  }
}

class Father2 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="father">
        Father2 {this.props.money.amount}
        <Son21 money={this.props.money}/>
        <Son22 money={this.props.money}/>
      </div>
    )
  }
}

class Son21 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son21 {this.props.money.amount}
      </div>
    )
  }
}

class Son22 extends React.Component {
  constructor() {
    super()
    // 监听别人是否花钱
    eventHub.on("我想花钱", (data)=>{
      this.setState({
        money: money
      })
    })
  }
  // 通知跑腿需要花钱
  x(){
    money.amount -= 100
    eventHub.trigger("我想花钱", 100)
    this.setState({
      money: money
    })
  }
  render() {
    return (
      <div className="son">
        son22 {this.props.money.amount}
        <button onClick={()=>this.x()}>花钱</button>
      </div>
    )
  }
}

render()

function render() {
  ReactDOM.render(<App/>, document.querySelector("#root"))
}
```

### 解析

[代码预览](https://jsbin.com/besuhof/5/edit?html,js,output)

提前跟跑腿的说明，每当son22花钱时，通知son11有人花钱了。从而更新信息。

> 每个人消费跑腿都需要来回通知效率很慢。需要实现告诉跑腿的要监听谁消费了。

## 方法二 eventHub（单向数据流）

找一个管家，每次消费都去向下通知所有成员。

### 代码

#### HTML

与上次一样的代码，这里就不重复了

#### CSS

与上次一样的代码，这里就不重复了

#### JSX

```jsx
// 财产
let money = {
  amount: 100000
}

// 事件中心
let fnLists = {}
let eventHub = {
  trigger(eventName, data) {
    let fnList = fnLists[eventName]
    if (!fnList) return
    for (let i = 0; i < fnList.length; i++) {
      fnList[i](data)
    }
  },
  on(eventName, fn) {
    if (!fnLists[eventName]) {
      fnLists[eventName] = []
    }
    fnLists[eventName].push(fn)
  }
}

// 管家
let x = {
  init() {
    eventHub.on("我想花钱", function (data) {
      money.amount -= data
      render()
    })
  }
}

x.init()

class App extends React.Component {
  constructor() {
    super()
    this.state = {
      money: money
    }
  }

  render() {
    return (
      <div className="home">
        <Father1 money={this.state.money}/>
        <Father2 money={this.state.money}/>
      </div>
    )
  }
}

class Father1 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="father">
        Father1 {this.props.money.amount}
        <Son11 money={this.props.money}/>
        <Son12 money={this.props.money}/>
      </div>
    )
  }
}

class Son11 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son11 {this.props.money.amount}
      </div>
    )
  }
}

class Son12 extends React.Component {
  constructor() {
    super()
  }

  x() {
    eventHub.trigger("我想花钱", 100)
  }

  render() {
    return (
      <div className="son">
        son12 {this.props.money.amount}
        <button onClick={() => this.x()}>花钱</button>
      </div>
    )
  }
}

class Father2 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="father">
        Father2 {this.props.money.amount}
        <Son21 money={this.props.money}/>
        <Son22 money={this.props.money}/>
      </div>
    )
  }
}

class Son21 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son21 {this.props.money.amount}
      </div>
    )
  }
}

class Son22 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son22 {this.props.money.amount}
      </div>
    )
  }
}

render()

function render() {
  ReactDOM.render(<App/>, document.querySelector("#root"))
}
```

### 解析

[预览地址](https://jsbin.com/besuhof/6/edit?html,js,output)

全部数据统一由管家来下发更新

## 方法三 redux（单向数据流）

与eventHub的思路一样

### HTML

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <script src="https://cdn.bootcdn.net/ajax/libs/react/17.0.0-rc.0/umd/react.production.min.js"></script>
  <script
    src="https://cdn.bootcdn.net/ajax/libs/react-dom/0.0.0-0c756fb-f7f79fd/umd/react-dom.production.min.js"></script>
  <script src="https://cdn.bootcdn.net/ajax/libs/redux/4.0.5/redux.min.js"></script>
</head>
<body>
<div id="root"></div>
</body>
</html>
```

### CSS

与上次一样的代码，这里就不重复了

### JSX

```jsx
// redux
let createStore = Redux.createStore
let reducers = (state, action) => {
  state = state || {
    money: {amount: 100000}
  }
  switch (action.type) {
    case "我想花钱":
      return {
        money: {
          amount: state.money.amount - action.payload
        }
      }
    default:
      return state
  }
}

const store = createStore(reducers)

class App extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="home">
        <Father1 money={this.props.store.money}/>
        <Father2 money={this.props.store.money}/>
      </div>
    )
  }
}

class Father1 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="father">
        Father1 {this.props.money.amount}
        <Son11 money={this.props.money}/>
        <Son12 money={this.props.money}/>
      </div>
    )
  }
}

class Son11 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son11 {this.props.money.amount}
      </div>
    )
  }
}

class Son12 extends React.Component {
  constructor() {
    super()
  }

  x() {
    store.dispatch({type: "我想花钱", payload: 100})
  }

  render() {
    return (
      <div className="son">
        son12 {this.props.money.amount}
        <button onClick={() => this.x()}>花钱</button>
      </div>
    )
  }
}

class Father2 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="father">
        Father2 {this.props.money.amount}
        <Son21 money={this.props.money}/>
        <Son22 money={this.props.money}/>
      </div>
    )
  }
}

class Son21 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son21 {this.props.money.amount}
      </div>
    )
  }
}

class Son22 extends React.Component {
  constructor() {
    super()
  }

  render() {
    return (
      <div className="son">
        son22 {this.props.money.amount}
      </div>
    )
  }
}

function render() {
  ReactDOM.render(<App store={store.getState()}/>, document.querySelector("#root"))
}

render()
store.subscribe(render)

```
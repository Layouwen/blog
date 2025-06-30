---
title: React如何实现父子组件通信
date: 2020-08-28 15:57
tags:
  - 博客
  - 前端
  - react
categories:
  - 博客
  - 前端
---

## 一、场景

在父组件显示数值，在子组件有个按钮点击后修改父组件的值。

## 二、代码

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
  <script src="https://cdn.bootcdn.net/ajax/libs/react-dom/0.0.0-0c756fb-f7f79fd/umd/react-dom.production.min.js"></script>
</head>
<body>
<div id="root"></div>
</body>
</html>
```

### CSS

```css
.father {
    background: #000;
}
.son {
    background: #333;
}
```

### JSX

```jsx
function App(){
  return (
    <div>
      <Father/>
    </div>
  )
}

class Father extends React.Component{
  constructor(){
    super()
    this.state = {
      number: 0
    }
  }
  changeValue(){
    this.setState({
      number: this.state.number + 1
    })
  }
  render(){
    return (
      <div className="father">
        <div>{this.state.number}</div>
        <Son change={this.changeValue.bind(this)}/>
      </div>
    )
  }
}

class Son extends React.Component {
  constructor(){
    super()
  }
  render(){
    return (
      <div className="son">
        <button onClick={this.props.change}>更改值</button>
      </div>
    )
  }
}

render()

function render(){
  ReactDOM.render(<App/>, document.querySelector('#root'))
}
```

## 三、结论

在父组件声明一个函数，用于修改父组件中的值。
将此函数通过子组件的props进行传参
```jsx
<Son change={this.changeValue.bind(this)}>
```
子组件接收后，即可调用外部函数
```js
<button onClick={this.props.change}>更改值</button>
```

## 四、代码效果预览

[预览链接](https://jsbin.com/fubefeg/11/edit?html,js,output)
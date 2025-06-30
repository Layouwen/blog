---
title: React的生命周期概括
date: 2020-09-01 15:50
tags:
  - 博客
  - react
  - 前端
categories:
  - 博客
  - 前端
---

##  一、生命周期

### getDefaultProps

> 设置默认的Props

### getInitialState

> 可以访问this.props

### componentWillMount

> 挂载页面前，在渲染前调用。此时还可以修改state

### render

> 渲染页面，此时已经不能更改state

### componentDidMount

> 挂在页面后调用

### componentWillUpdate

```jsx
componentWillUpdate(nextPorps, nextState)
```

> 组件数据更新时调用

### shouldComponentUpdate

```jsx
shouldComponentUpdate(nextPorps, nextState)
```

> 当props或state更新时触发，用来判断数值是否发生变化，并返回布尔值。true为进行更新，false为阻止更新

### componentDidUpdate

> 组件跟新完毕后调用，此时可以修改state值

### componentWillUnmount

> 组件卸载时调用，一般用于清除事件监听和定时器

### componentWillReceiveProps

```jsx
componentWillReceiveProps(nextProps)
```

> 当外部传来的props发生变化时触发，它接收变更后的props值可供使用
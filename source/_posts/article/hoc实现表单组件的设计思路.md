---
title: hoc实现表单组件的设计思路
date: 2021-08-20 10:03
tags:
  - 博客
  - 前端
  - react
categories:
  - 博客
  - 前端
---

## 前言

通过模仿 rc-form 的实现思路，可以学习一下 hoc 的使用场景。

## 暴露 createForm() 函数

通过 createForm 函数，返回一个组件。该组件拓展了我们的一些方法。

```js
export function createForm(Cmp) {
  return class extends Component {
    getFieldDecorator = () => {}
    
    getFieldsValue = () => {}
    
    getFieldValue = () => {}
    
    setFieldValue = () => {}
    
    validateFields = () => {}
  
    form = () => {
      return {
        getFieldDecorator: this.getFieldDecorator,
        getFieldValue: this.getFieldValue,
        getFieldsValue: this.getFieldsValue,
        setFieldValue: this.setFieldValue,
        validateFields: this.validateFields,
      }
    }
    
    render() {
      const form = getForm()
      return <Cmp {...this.props} form={form} />
    }
  }
}
```

## 实现 getFieldDecorator

使用方法

```js
<div>
  {getFieldDecorator('username', {rules: { require: true, message: '请输入用户名' })(<input placeholder="请输入用户名" />)}
</div>
```

可以看出，该函数接收两个参数，第一个是字段名，第二个是它的规则。又接着返回了一个接收一个组件的函数，最终返回一个加工后的组件。现在我们开始简单实现一下。

```js
constructor(){
  // ...
  this.state = {}
  this.options = {}
}
```

```js
getFieldDecorator = (fieldName, option) => InputCmp => {
  // 保存数据和选项
  if (this.state[fieldName] === undefined) this.setState({ [fieldName]: '' })
  this.options[fieldName] = option
  // 返回一个处理后的组件
  return React.cloneElement(InputCmp, {
    name: fieldName,
    value: this.state[fieldName],
    onChange: this.handleChange,
  })
}
```

定义 handleChange 事件

```js
handleChange = (e) => {
  const { name, value } = e.target
  this.setState({ [name]: value })
}
```

## 实现 getFieldValue

直接把 state 的数据返回即可

```js
getFieldsValue = () => {
  return {...this.state}
}
```

## 实现 getFieldsValue

通过传进来的名字，返回对应的数据

```js
getFieldValue = name => {
  return this.state[name]
}
```

## 实现 setFieldValue

直接将传进来的数据合并起来即可

```js
setFieldValue = state => {
  this.setState(state)
}
```

## 实现 validateFields

该方法接收一个回调函数，通过遍历options的规则，在判断相应的值，返回错误数据以及state的数据

```js
validateFields = callback => {
  const err = []
  for (let fieldName in this.options) {
    const rules = this.options[fieldName]?.rules
    const value = this.state[fieldName]
    if(rules && rules.require && rules.message && !value) {
      err.push({
        [fieldName]: rules.message
      })
    }
  }
  // 判断 err 是否有数据
  if (err.length === 0) {
    callback(null, { ...this.state })
  } else {
    callback(err, { ...this.state })
  }
}
```

## 最终代码

```js
import React, { Component } from 'react'

export function createForm(Cmp) {
  return class extends Component {
    constructor(props) {
      super(props)
      this.state = {}
      this.options = {}
    }

    handleChange = e => {
      const { name, value } = e.target
      this.setState({ [name]: value })
    }

    validateFields = callback => {
      const err = []
      for (let fieldName in this.options) {
        const rules = this.options[fieldName]?.rules
        if (rules && rules.require && rules.message && !this.state[fieldName]) {
          err.push({
            [fieldName]: rules.message,
          })
        }
      }
      if (err.length === 0) {
        callback(null, { ...this.state })
      } else {
        callback(err, { ...this.state })
      }
    }

    getFieldDecorator = (fieldName, option) => InputCmp => {
      this.options[fieldName] = option
      if (this.state[fieldName] === undefined) this.setState({ [fieldName]: '' })

      return React.cloneElement(InputCmp, {
        name: fieldName,
        value: this.state[fieldName],
        onChange: this.handleChange,
      })
    }

    getFieldValue = name => {
      return this.state[name]
    }

    getFieldsValue = () => {
      return { ...this.state }
    }

    setFieldValue = state => {
      this.setState(state)
    }

    getForm = () => {
      return {
        getFieldDecorator: this.getFieldDecorator,
        getFieldValue: this.getFieldValue,
        getFieldsValue: this.getFieldsValue,
        setFieldValue: this.setFieldValue,
        validateFields: this.validateFields,
      }
    }

    render() {
      const form = this.getForm()
      return <Cmp {...this.props} form={form} />
    }
  }
}
```
---
title: react使用AntD
date: 2020-09-03 16:32
tags:
  - 博客
  - react
  - 前端
  - antd
categories:
  - 博客
  - 前端
---

## 一、安装AntD

```bash
yarn add antd
```

## 二、引入AntD的CSS样式

```bash
import "antd/dist/antd.css"
```

## 三、在官方文档寻找自己需要的组件并使用

### 例如：Button

1. 先import组件到页面
```js
import { Button } from "antd"
```

2. 直接使用
```js
function Demo(){
    return (
        <>
            <Button type="primary">主按钮</Button>
            <Button>次按钮</Button>
        </>
    )
}
```

### 例如：Form

1. 先import组件到页面
```js
import { Form, Input, Button, checkbox } from "antd"
```

2. 直接使用
```jsx
const layout = {
    labelCol: { span: 8 },
    wrapperCol: { span: 16 }
}
const tailLayout = {
    wrapperCol: { offset: 8, span: 16 }
}

const Demo = () => {
  const onFinish = values => {
    console.log("Success:", values)
  }

  const onFinishFailed = errorInfo => {
    console.log("Failed:", errorInfo)
  }

  return (
    <Form
      {...layout}
      name="basic"
      initialValues={{
        remember: true,
      }}
      onFinish={onFinish}
      onFinishFailed={onFinishFailed}
    >
      <Form.Item
        label="Username"
        name="username"
        rules={[
          {
            required: true,
            message: "Please input your username!",
          },
        ]}
      >
        <Input/>
      </Form.Item>

      <Form.Item
        label="Password"
        name="password"
        rules={[
          {
            required: true,
            message: "Please input your password!",
          },
        ]}
      >
        <Input.Password/>
      </Form.Item>

      <Form.Item {...tailLayout} name="remember" valuePropName="checked">
        <Checkbox>Remember me</Checkbox>
      </Form.Item>

      <Form.Item {...tailLayout}>
        <Button type="primary" htmlType="submit">
          Submit
        </Button>
      </Form.Item>
    </Form>
  )
}
```

3. 修改里面的参数达到自己的效果

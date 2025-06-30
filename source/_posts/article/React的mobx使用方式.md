---
title: React的mobx使用方式
date: 2020-09-03 17:22
tags:
  - 博客
  - react
  - 前端
  - mobx
categories:
  - 博客
  - 前端
---

## 一、创建文件

stores
----- auth.js 例子文件
----- index.js 总入口

## 二、安装

```bash
yarn add mobx
yarn add mobx-react
```

## 三、书写相应代码

### auth.js

1. import相应的文件
```js
import { observable, action } from "mobx"
```

2. 声明class类，并导出
```js
class AuthStore {
    @observable 变量名 = 值
    @boservable values = {
        username = "梁又文"
        sex = "男"
    }
    
    @action 方法名(参数) {
        console.log(参数)
    }
    @action setName(name){
        this.values.username = name
    }
}

export { AuthSotre }
```

### index.js

1. import相应文件
```js
import React, { createContext, useContext } from "react"
import { 类名 } from "../stores/文件名"
import { AuthSotre } from "../stores/auth"
```

2. 创建Context对象
```js
const context = createContext({
    定义方法名: new 类名(),
    authStore: new AuthStore()
})
```

3. 将Context对象全局导出
```js
export const useStores = () => useContext(context)
```

## 四、配置package.json

1. 将react隐藏的webpack暴露出来，释放之前请先提交代码
```bash
yarn eject
```

2. 安装插件
```js
yarn add @babel/plugin-proposal-decorators
```

3. 修改package.json
```json
"babel": {
    "plugins": [
        ["@babel/plugin-proposal-decorators", {"legacy": true}]
    ],
    "presets": [
        "react-app"
    ]
}
```

## 五、在组件中进行使用

1. import相应文件
```js
import { observer } from "mobx-react"
import { useStores } from "../stores"
```

2. 使用observer监控组件，并解构我们需要的对象出来并使用
```js
const Demo = observer(()=>{
    const { AuthStore } = useStores()
   
    return (
        <>
            <h1>我是Demo组件</>
        </>
    )
})
```

3. 解构后我们就可以使用该对象的属性及方法
```js
const Demo = observer(()=>{
    const { AuthStore } = useStores()
    
    const changeName = () => {
        AuthStore.setName("改名字后的梁又文")
    }
   
    return (
        <>
            <h1>我是Demo组件，我的名字叫{ AuthStore.values.username }</h1>
            <button onClick={changeName}>改名字</button>
        </>
    )
})
```
---
title: React如何使用Hooks
date: 2020-09-01 15:11
tags:
  - 博客
  - react
  - 前端
  - hooks
categories:
  - 博客
  - 前端
---

## 一、State

### 步骤

1. 导入useState

```js
import React, {useState} from "react"
```

2. 声明State

```js
let [value, setValue] = useState(0)
```

> 数组第一个参数为使用的值，第二个参数为设置值的函数。useState的参数为默认值。

3. 使用值

```html
<div>
    <span>{value}</span>
</div>
```

4. 设置值、

```js
setValue(value + 1)
```

> 参数即为对值得变更操作

### 代码示例

```jsx
import React, { useState } from "react";

function App() {
    let [value, setValue] = useState(0);
    const add1 = () => {
        setValue(value + 1);
    };
    return (
        <div>
            <span>{value}</span>
            <button onClick={add1}>+1</button>
        </div>
    );
}
export default App;
```

## 二、useReducer

### 步骤

1. 导入useReducer

```js
import { useReducer } from "react"
```

2. 创建初始值

```js
const initial = {
    n: 0
}
```

3. 创建所有操作类型

```js
const reducer = (state, action) => {
    if (action.type === "add") {
        return { n: state.n + action.number }
    } else if (action.type === 'multi') {
        return { n: state.n - action.number }
    } else {
        throw new Error("未知类型")
    }
}
```

4. 使用useReducer，获得读写操作

```js
const [state, dispatch] = useReducer(action, initial)
```

读
```html
<div>{state.n}</div>
```

写
```html
dispath(type: "add", numer: 1 )
```

### 代码示例

```jsx
import React from "react"
import { useReducer } from "react"

const initial = {
    n: 0
}

const reducer = (state, action) => {
    if (action.type === 'add') {
        return { n: state.n + action.number }
    } else if (action.type = "multi") {
        return { n: state.n - action.number }
    } else {
        throw new Error("未知类型")
    }
}

const DemoUseReducer = () => {
    const [state, dispatch] = useReducer(reducer, initial)
    const add = () => {
        dispatch({ type: "add", number: 1 })
    }
    return (
        <>
            <div>{state.n}</div>
            <button onClick={add}>+1</button>
            <button onClick={() => dispatch({ type: "multi", number: 3 })}>-3</button>
        </>
    )
}

export default DemoUseReducer
```

## 三、useContext

### 步骤

1. 导入createContext、useContext

```js
import React, { useState, createContext, useContext } from "react"
```

2. 创建Context

```jsx
const Context = createContext(null)
```

3. 设置作用域，传递你需要使用的数据

```jsx
return (
    <Context.Provider value={{n, setN}}>
        <Father/>
        <Son/>
    </Context.Provider>
)
```

4. 在作用域中的组件解构传递的数据

Father
```jsx
const {n, setN} = useContext(Context)
```

5. 使用解构出来的数据

Father
```html
<button onClick={()=>setN(n=>n+1)}>+1</button>
```

### 代码示例

```jsx
import React, { useState, createContext, useContext } from "react"

const Context = createContext(null)

const DemoUseContext = () => {
    const [n, setN] = useState(0)
    return (
        <Context.Provider value={{ n, setN }}>
            <Father />
            <Son />
        </Context.Provider>
    )
}

const Father = () => {
    const { n, setN } = useContext(Context)
    return (
        <>
            <div>我是爸爸{n}</div>
            <button onClick={() => setN(n => n + 1)}>爸爸按钮+1</button>
        </>
    )
}

const Son = () => {
    const { n, setN } = useContext(Context)
    return (
        <>
            <div>我是儿子{n}</div>
            <button onClick={() => setN(n => n - 1)}>儿子按钮-1</button>
        </>
    )
}

export default DemoUseContext
```

## 四、useEffect和useLayoutEffect

### 步骤

1. 导入

```js
import { useEffect, useLayoutEffect } from "react"
```

2. 每次都执行

```js
useEffect(()=>{})
```

3. 第一次渲染执行

```js
useEffect(()=>{},[])
```

4. 在某个值变化的时候执行

```js
useEffect(()=>{},[n])
```

5. 在页面渲染前执行

```js
useLayoutEffect(()=>{})
```

### 代码示例

```jsx
import React, {useState, useEffect, useLayoutEffect} from "react"

const DemoUseEffect = () => {
  const [display, setDisplay] = useState(true)
  const [n, setN] = useState(0)
  useEffect(() => {
    console.log("我每次都执行")
  })
  useEffect(() => {
    console.log("我只在第一次执行")
    return () => {
      console.log("我只在销毁的时候执行")
    }
  }, [])
  useEffect(() => {
    console.log("我只在n变化执行")
  }, [n])
  useLayoutEffect(() => {
    console.log("我是在页面渲染前就执行结束")
  })
  return (
    <>
      {display ? <div>{n}</div> : null}
      <button onClick={() => setN(n => n + 5)}>+5</button>
      <button onClick={() => {
        setDisplay(display => !display)
      }}>消灭n
      </button>
    </>
  )
}

export default DemoUseEffect
```

## 五、memo&useMemo&useCallback

### 步骤

1. 导入

```js
import React, {useMemo, useState, memo, useEffect, useCallback} from "react"
```

2. memo包住不需要重新渲染的组件函数

```js
const Childer = memo((props) => {
  console.log("我是孩子，我不想执行")
  return (
    <>
      <div>我是孩子 {props.childer}</div>
    </>
  )
})
```

3. useMemo包住防止因对象地址变化而导致的误渲染

```js
const childClick = useMemo(() => {
  return () => {
  }
}, [childer])
```


### 代码示例

```jsx
import React, {useMemo, useState, memo, useEffect, useCallback} from "react"

const DemoUseMemoAndUseCallback = () => {
  const [n, setN] = useState(0)
  const [childer, setChilder] = useState(0)
  useEffect(() => {
    console.log("我变化了")
  }, [n])
  // 使用useMemo阻止因为对象地址变化而重新执行
  const childClick = useMemo(() => {
    return () => {
    }
  }, [childer])
  // 等同于useMemo，比useMemo简单
  const childClick2 = useCallback(() => {
  })
  return (
    <>
      <div>{n}</div>
      <Childer childer={childer} childClick={childClick} childClick2={childClick2}/>
      <button onClick={() => setN(n => n + 10)}>+10</button>
    </>
  )
}

// 使用memo阻止子组件state没改变，因父组件属性改变而重新渲染
const Childer = memo((props) => {
  console.log("我是孩子，我不想执行")
  return (
    <>
      <div>我是孩子 {props.childer}</div>
    </>
  )
})

export default DemoUseMemoAndUseCallback
```

## 六、useRef

### 步骤

1. 导入

```js
import { useRef } from "react"
```

2. 声明变量

```js
const count = useRef(0)
```

3. 使用/修改值

```js
count.current += 1
```

### 代码示例

```js
import React, {useEffect, useRef, useState} from "react"

const DemoUseRef = () => {
  const count = useRef(0)
  const [n, setN] = useState(0)
  useEffect(() => {
    count.current += 1
    console.log("第" + count.current + "执行")
  })
  return (
    <>
      <div>{n}</div>
      <button onClick={() => setN(n => n + 1)}>n+1</button>
    </>
  )
}

export default DemoUseRef
```

## 七、useImperativeHandle

### 步骤

1. 导入

```js
import { useImperativeHandle } from "react"
``` 

2. 在父组件创建ref

```js
const buttonRef = useRef()
```

3. 传递给子组件

```html
<button ref={buttonRef}>按钮</button>
```

4. 子组件接收并对ref进行修改后返还出去

```js
useImperativeHandle(ref, ()=>{
    return {
        x: ()=>console.log(1)
    }
})
```

### 代码示例

```jsx
import React, {forwardRef, useRef, useEffect, useImperativeHandle} from "react"

const DemoImperativeHandle = () => {
  const buttonRef = useRef()
  useEffect(() => {
    console.log(buttonRef)
  })
  return (<>
    <Son ref={buttonRef} onClick={() => console.log(buttonRef.current.x())}>按钮</Son>
  </>)
}

const Son = forwardRef((props, ref) => {
  const realRef = useRef()
  useImperativeHandle(ref, () => {
    return {
      x: () => {
        console.log(1)
      },
      ref: realRef
    }
  })
  return (<>
    <button ref={realRef} {...props}/>
  </>)
})

export default DemoImperativeHandle
```

## 八、forwardRef

### 步骤

1. 导入

```js
import { forwardRef, useRef } from "react"
```

2. 创建ref

```js
const ref = useRef(null)
```

3. 向组件传ref

```html
<ChildNode ref={ref}>按钮</ChildNode>
```

4. 使用forwardRef接收ref

```jsx
const ChildNode = forwardRef((props, ref) => {
  return (
    <>
      <button ref={ref} onClick={() => console.log(ref)}>{props.children}</button>
    </>
  )
})

```

### 代码示例

```jsx
import React, {forwardRef, useRef} from "react"

const DemoForwardRef = () => {
  const ref = useRef(null)
  return (
    <>
      <div>我是本身的元素</div>
      <ChildNode ref={ref}>按钮</ChildNode>
    </>
  )
}

const ChildNode = forwardRef((props, ref) => {
  return (
    <>
      <button ref={ref} onClick={() => console.log(ref)}>{props.children}</button>
    </>
  )
})

export default DemoForwardRef
```

## 拓展一、useContext&useReducer代替Redux

### 步骤

1. 创建Store数据仓库

```js
cosnt store = {user: null, books: null, movies}
```

2. 创建reducer行为操作列表

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "setUser":
      return {...state, user: action.user}
    case "setBooks":
      return {...state, books: action.books}
    case "setMovies":
      return {...state, movies: action.movies}
    default:
      throw new Error("位置类型")
  }
}
```

3. 创建Context

```js
const Context = createContext(null)
```

4. 创建读写的API

```js
const [state, dispatch] = useReducer(reducer, store)
```

5. 定义作用域

```html
<Context.Provider value={{state, dispatch}}>
  <User/>
  <Books/>
  <Movies/>
</Context.Provider>
```

6. 使用传递的数据

```js
const {state, dispatch} = useContext(Context)
```

7. 对数据进行操作

读
```html
<div>{state.user.name}</div>
```

写
```js
dispatch({type: "setUser", user: 数据})
```

### 代码示例

```jsx
import React, {useContext, useEffect, useReducer, createContext} from "react"

// 数据仓库
const store = {
  user: null,
  books: null,
  movies: null
}

// 行为类型
const reducer = (state, action) => {
  switch (action.type) {
    case "setUser":
      return {...state, user: action.user}
    case "setBooks":
      return {...state, books: action.books}
    case "setMovies":
      return {...state, movies: action.movies}
    default:
      throw new Error("位置类型")
  }
}

// 创建Context
const Context = createContext(null)

const DemoContextReducer = () => {
  // 创建数据读写的API
  const [state, dispatch] = useReducer(reducer, store)
  return (
    <Context.Provider value={{state, dispatch}}>
      <User/>
      <Books/>
      <Movies/>
    </Context.Provider>
  )
}

const User = () => {
  const {state, dispatch} = useContext(Context)
  useEffect(() => {
    ajax("/user").then(user => {
      dispatch({type: "setUser", user})
      console.log(user)
    })
  }, [])
  return (
    <>
      <h3>姓名</h3>
      {state.user ? <div>{state.user.name}</div> : null}
    </>
  )
}
const Books = () => {
  const {state, dispatch} = useContext(Context)
  useEffect(() => {
    ajax("/books").then(books => {
      dispatch({type: "setBooks", books})
    })
  }, [])
  return (
    <>
      <h3>书籍</h3>
      {state.books ? state.books.map(book => (<div key={book.id}>{book.name}</div>)) : null}
    </>
  )
}
const Movies = () => {
  const {state, dispatch} = useContext(Context)
  useEffect(() => {
    ajax("/movies").then(movies => {
      dispatch({type: "setMovies", movies: movies})
    })
  }, [])
  return (
    <div>
      <h3>电影</h3>
      {state.movies ? state.movies.map(item => <div key={item.id}>{item.name}</div>) : null}
    </div>
  )
}

export default DemoContextReducer

// 模拟请求数据
function ajax(path) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (path === "/user") {
        resolve({
          id: 1,
          name: "梁又文"
        })
      } else if (path === "/books") {
        resolve([{
          id: 1,
          name: "我是一本好书"
        }, {
          id: 2,
          name: "我是一本坏书"
        }])
      } else if (path === "/movies") {
        resolve([{
          id: 1,
          name: "最时间的尽头"
        }, {
          id: 2,
          name: "八百"
        }])
      }
    }, 3000)
  })
}
```

## 拓展二、自定义hook

### 代码示例

#### useList

```jsx
import {useState} from "react"

const useList = () => {
  const [list, setList] = useState(0)
  return ({
    list: list,
    setList: setList
  })
}

export default useList
```

#### DemoCustomHook

```jsx
import React from "react"
import useList from "./hooks/useList"

const DemoCustomHook = () => {
  const {list, setList} = useList()
  return (
    <>
      <div>{list}</div>
      <button onClick={() => setList(n => n + 10)}>+10</button>
    </>
  )
}

export default DemoCustomHook
```
---
title: 学Vue3核心概念与面试官斗智斗勇（一） 收集触发依赖
date: 2022-03-14 01:09
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
  - vue
---

> 本文章依据阅读源码的理解进行编写。如果有什么错误的地方，欢迎指正交流学习。  
> 最近也在帮助想入行前端的朋友进行学习，如果有需要交流学习，可以添加微信 **gdgzyw**。  
> 聊天、学习、打游戏都阔以~

学习源码最快的方式就是理解概念后，自己写一个简版的功能。所以我们得先搭一个环境，这里采用测试驱动的方式进行。

## 初始化项目

初始化 `package.json` 和安装依赖

```bash
yarn init -y
yarn add -D @babel/core @babel/preset-env @babel/preset-typescript @types/jest babel-jest jest
```

添加 `scripts` 用于启动 `jest`

**package.json**
```json
{
  // ...
  "scripts": {
    "test": "jest"
  },
  // ...
}
```

根目录创建 `babel.config.js`

```js
module.exports = {
  presets: [['@babel/preset-env', { targets: { node: 'current' } }], '@babel/preset-typescript'],
}
```

创建 `tsconfig.json` 文件

```json
{
  "compilerOptions": {
    "target": "es2016",
    "lib": [
      "DOM",
      "es6"
    ],
    "module": "commonjs",
    "types": [
      "jest"
    ],
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitAny": false,
    "skipLibCheck": true
  }
}
```

至此我们的项目就初始化完成了。如果你需要用 **git** 来管理，可以自行 `git init`。

## 编写测试用例

创建 `src/reactivity/tests/effect.spec.ts` 文件

```ts
describe('effect', () => {
  it('happy path', () => {
    const bank = reactive({
      money: 100,
    });

    let myMoney;
    effect(() => {
      myMoney = bank.money * 2;
    });
    expect(myMoney).toBe(200);
    bank.money = 50;
    expect(myMoney).toBe(100);
  });
});
```

创建 `scr/reactivity/tests/reactive.spec.ts`

```ts
describe('reactive', () => {
  it('happy path', () => {
    const origin = {money: 100};
    const bank = reactive(origin);
    expect(bank).not.toBe(origin);
    expect(bank.money).toBe(100);
  });
});
```

现在我们运行 `yarn test` 测试用例是跑不通的。对应的函数我们还没有创建。下面正式开始我们的编码环节。

## 编写 reactive 函数

通过上面的测试用例，我们可以知道，我们接收一个对象的值，并且对他进行一个拦截。所以我们可以直接返回一个 proxy 的代理对象。

```ts
export function reactive(obj) {
  return new Proxy(obj, {
    get(target, key) {
      return Reflect.get(target, key)
    },
    set(target, key, value) {
      return Reflect.set(target, key, value)
    },
  })
}
```

> Reflect.get(target, key) 等同于 target[key]

接着我们为了统一出口可以创建 `src/reactivity/index.ts` 文件

**index.ts**

```ts
export * from './reactive'
```

接着我们去 `reactive.spec.ts` 中引入我们的函数

```ts
import {reactive} from '../index'
```

跑一下测试 

```bash
yarn test reactive
```

提示 `PASS` 至此发现这个的单侧已经跑通。接着我们可以开始写另一个单测。

## 编写 effect 函数

一样的，我们观察一下测试用例的参数。  
可以发现他接受一个回调，所以我们参数是一个回调函数。  
接着我们思考一下如何将我们上一个 reactive 的函数与这个回调函数产生关联。   

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/72aedf5acd77448f91cc5ca941dd9b5f~tplv-k3u1fbpfcp-zoom-1.image)

定义 tagetMap 变量，用于对象的分组。  
定义 depsMap 变量，用于对象中每个 key 的依赖分组。  
通过 reactive 定义对象，在 get 的时候，我们在 targetMap
 中将对象添加到 Map 中作为分类。接着创建 Set 用 key 作为分类保存到 Set 中。  
 
回顾我们的单侧流程，我们先定义了个 reactive 对象。  
接着我们在 effect 函数中执行了回调函数，回调函数中我们会读取到 reactive 的值，从而触发了 get 操作。所以我们需要在 get 操作中进行依赖收集。

定义一个 ReactiveEffect 类，收集我们的回调函数

**src/reactivity/effect.ts**
```ts
let activeEffect
class ReactiveEffect {
  private readonly _fn: any
  constructor(fn) {
    this._fn = fn
  }
  run() {
    activeEffect = this
    this._fn()
  }
}

export function effect(fn) {
  const _effect = new ReactiveEffect(fn)
  _effect.run()
}
```

> 在初始化的时候，我们保存回调函数到 _fn 中，在我们执行 run 方法的时候。会触发我们的回调函数。

定义一个 track 的函数，完成收集依赖这个操作

**src/reactivity/effect.ts**
```ts
const targetMap = new Map()
export function track(target, key) {
  let depsMap = targetMap.get(target)
  if (!depsMap) {
    depsMap = new Map()
    targetMap.set(target, depsMap)
  }
  let deps = depsMap.get(key)
  if (!deps) {
    deps = new Set()
    depsMap.set(key, deps)
  }
  deps.add(activeEffect)
}
```
 
如果此时我们需要设置 reactive 的值，我们会触发 set 操作。所以触发依赖的操作需要在 set 中进行。

定义 trigger 函数，触发所有依赖

```ts
export function trigger(target, key) {
  const depsMap = targetMap.get(target)
  const deps = depsMap.get(key)
  for (const dep of deps) {
    dep.run()
  }
}
```

回到 reactive 文件，将 track 和 trigger 写到对应的操作中。

**src/reactivity/index.ts**
```
// ...
export * from './effect'
```

**src/reactivity/reactive.ts**
```ts
import {target, trigger} from './index'

export function reactive(obj) {
  return new Proxy(obj, {
    get(target, key) {
      track(target, key)
      return Reflect.get(target, key)
    },
    set(target, key, value) {
      const res = Reflect.set(target, key, value)
      trigger(target, key)
      return res
    },
  })
}
```

回到我们的单侧，将这几个库引入

**src/reactivtiy/tests/effect.spec.ts**

```ts
import {effect,reactive} from '../index' 

// ...
```

至此我们收集依赖和触发依赖的核心逻辑已经实现。我们现在可以跑 `yarn test` 进行检验。

## 结语

这篇文章是这个系列的开始，后续我会继续分享相关内容。慢慢完善我们对 vue3 的理解。  
欢迎关注我，与我深入♂沟通。
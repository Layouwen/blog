---
title: JavaScript 篇
date: 2021-03-22T23:18:34.000Z
tags:
  - 汇总
  - 知识点总结
  - javascript
categories:
  - 汇总
  - 知识点总结
uuid: 6adaa889-3d04-434d-b084-628886887eb6
---

## 1、使用过哪些ES6语法

let、const、箭头函数、计算属性名、拓展运算符、解构赋值、export、import、export default、Promise、Reflect

### 1.1 Promise

表示异步操作最终完成或失败，及其结果
成功调用resolve、失败reject
在Permise声明后可以使用.then进行代码处理。
第一个函数表示成功，第二个函数表示失败。
因为.then和.catch都是返回promise，所以可以被链式调用

- promise.all 当所有成功是才调用触发成功，否则触发失败
- promise.allSettled 当所有premise完成后，返回一个对象，对象里面有每个的结果，无论成功还是失败
- promise.any 接受一个promise集合，当其中一个成功，就返回那个成功的promise值
- promise.race 其中一个成功或失败，则返回该成功或失败的值

#### 1.1.1 实现简易Promise

```js
class Promise2 {
  queue1 = []
  queue2 = []
  constructor(fn) {
    const resolve = data => {
      setTimeout(() => {
        for (let i = 0; i < this.queue1.length; i++) {
          this.queue1[i](data)
        }
      })
    }
    const reject = reason => {
      setTimeout(() => {
        for (let i = 0; i < this.queue2.length; i++) {
          this.queue2[i](reason)
        }
      })
    }

    fn(resolve, reject)
  }
  then(s, e) {
    this.queue1.push(s)
    this.queue2.push(e)
    return this
  }
}

p1 = new Promise2((resolve, reject) => {
  if (Math.random() > 0.5) {
    resolve('成功')
  } else {
    reject('失败')
  }
})

p1.then(res=>console.log(res), err=>console.log(err))
```

#### 1.1.2 实现 Promise.allSettled

```js
const p1 = new Promise((resolve, reject)=>{
  setTimeout(()=>{
    resolve("成功了")
  },2000)
})

const p2 = new Promise((resolve, reject)=>{
  setTimeout(()=>{
    reject("失败了")
  },3000)
})

const myAllSettled = (list)=>{
  // 收集需要返回的数组
  const resArr = new Array(list.length)
  // 判断时候获取所有的 Promise 结果
  let num = 0
  return new Promise((resolve, reject)=>{
    // 遍历 Promise 数组
    list.forEach((item, key)=>{
      let obj = {}
      item.then(res=>{
        obj['status'] = 'fulfilled'
        obj['value'] = res
        resArr[key] = obj
        num++
        if(num===list.length)resolve(resArr)
      },err=>{
        obj['status'] = 'rejected'
        obj['reason'] = err
        resArr[key] = obj
        num++
        if(num===list.length)resolve(resArr)
      })
    })
  })
}

myAllSettled([p1,p2]).then(res=>{
  console.log(res)
})
```

### 1.2 Reflect

- Reflect.has(对象名, 属性名) 查看指定对象是否含有该属性
- Reflect.ownKeys(对象名) 列出该对象所有属性
- Reflect.set(对象名, 属性名, 值) 设置该对象的属性和值

## 2、防抖和节流

### 2.1 节流

相当于有冷却时间，冷却时间到了才允许执行下一次操作

```js
// 声明节流函数
function throttle(fn, delay){
  // 控制开关
  let useCan = true
  // 返回可执行函数
  return function(){
    // 判断是否可以使用
    if(useCan){
      // 执行函数
      fn.apply(this, arguments)
      // 执行结束后，关闭开关
      useCan=false
      // 时间到后开启开关
      setTimeout(()=>useCan=true, delay)
    }
  }
}
```

### 2.2 防抖

如果短时间执行多次，那么我们带着上一个内容一起执行

```js
// 声明一个防抖函数
function debounce(fn, delay){
  // 记录计时器Id
  let timerId = null
  // 返回一个可执行函数
  return function(){
    // 保存this
    const context = this
    // 每次执行判断是否存在计时器，如果存在，先把上一次的清除，重新计时
    if(timerId){window.clearTimeout(timerId)}
    // 将新的计时器Id保存下来
    timerId = setTimeout(()=>{
      // 需要执行的代码
      fn.apply(context, arguments)
      // 当代码执行结束后，重置计时器Id
      timerId = null
    },delay)
  }
}
```

## 3、手写AJAX

```js
// 详细版
var request = new XMLHttpRequest()
request.open('GET', '/a/b/c?name=layouwen', true)
request.onreadystatechange = function(){
  if(request.readyState === 4 && request.status === 200) {
    console.log(request.responseText)
  }
}
request.send()

// 精简版
var request = new XMLHttpRequest()
request.open('GET','a/b/c?name=layouwen', true)
request.onload = ()=> console.log(request.responseText)
request.send()
```

## 4、this指向问题

- fn() —— windows
- obj.fn() —— obj
- fn.call(xxx) —— xxx
- fn.apply(xxx) —— xxx
- fn.bind(xxx) —— xxx
- new Fn() —— new出来后的对象
- fn = ()=>{} —— 箭头函数外的一层

## 5、什么是闭包,它有什么作用

外层函数调用后，外层作用域对象被内层函数的作用域引用，无法释放。
作用：
隐藏变量，私有变量
闭包可以使其不会被垃圾回收。
缺点：
在IE中使用过多会造成内存泄露
清除闭包：
将引用着作用域链的变量设置为 null

## 6、什么是立即执行函数

声明一个匿名函数，接着马上执行它

```js
( function(){console.log(1)} )()
( function(){console.log(2)}() )
!function(){console.log(3)}()
+function(){console.log(4)}()
-function(){console.log(5)}()
~function(){console.log(6)}()
void function(){console.log(7)}()
new function(){console.log(8)}()
```

作用：
创建一个局部的作用域，外面访问不到防止变量污染

## 7、JSONP/CORS/跨域

因为浏览器不允许两个源不相同的互相请求。
源=协议+域名+端口号
只要有一样不相同就是不同源。

### 7.1 CORS

CORS跨源资源共享，它可以解决两个不同源相互访问。
使用方法也很简单，在后端提前设置好Access-Control-Allow-Origin即可。
如果为 * 号则表示可以任何人访问。
如果需要指定源访问，可以在后面接上允许的源地址。
比如：Access-Control-Allow-Origin：http://foo.example

### 7.2 JSONP

JSONP也是一种跨域解决方案，由于IE之前的版本不支持CORS进行跨域。所以我们需要想别的解决思路，就出现了JSONP。它与JSON没有关系，之所以这样叫是因为最初里面数据写的格式大多为JSON格式。
使用方法

### 7.3 sessionStorage和localstorage能跨域拿到吗？

localstorage只有在同源的情况下可以获取数据
sessionStorage不同页面和标签页面无法共享数据

#### 7.3.1 localStorage跨域二级域名共享方法

挂载一个隐藏的iframe加载顶级域名的代理页面，统一在顶级域名里操作localStorage，其他二级或n及域名读取或设置localStorage通过此ifarme来操作

顶级域名的html
```html
<script>
    document.domain = 'lyw.com' // 设置顶级域名
    const setItem = (key, data) => localStorage.setItem(key, data)
    const getItem = key => localStorage.getItem(key)
</script>
```

其他二级或n级域名
```html
<iframe style="display:none;" id="ifrStorageProxy" src="http://www.lyw.com/storageproxy.html"></iframe>
<script>
  document.domain = 'lyw.com'
  const winifrStorageProxy = document.getElementById('ifrStorageProxy').contentWindow
  winifrStorageProxy.onload = () => {
    winifrStorageProxy.setItem('showbo', 'test-' + newDate().toLocaleString()
    alert(winifrStorageProxy.setItem('showbo'))
  }
</script>
```

#### 7.3.2 sessioniStorage和localStorage跨顶级域名共享方法

与二级域名类似，任选一个域名挂载iframe代理页面，数据存处在此顶级域名下，其他顶级域名通过此代理读取localStorage和sessionStorage的数据

代理的html
```html
<script>
  window.addEventListener('message', e =>{
    if (e.source !== parent) return // 如果不是父页面发送的信息就返回
    let data = JSON.parse(e.data)
    switch (data.action) {
      case 'setItem':
        localStorage.setItem(data.key, data.data)
        break
      case 'getItem':
        data = localStorage.getItem(data.key)
        parent.postMessage(data, '*')
        break
    }
  }, false)
<script>
```

其他顶级域名页面
```html
<iframe style="display: none;" if="ifrStorageProxy" src="http://www.lyw.com/storageproxy.html"></iframe>
<script>
  let winifrStorageProxy = document.getElementById('ifrStorageProxy')
  window.addEventListener('message', e => {
    if (e.soure !== winifrStorageProxy.contentWindow) return // 不是我们自己iframe发送的信息就返回
    alert(e.data)
  }, false)
  // 要在加载完毕后进行操作，不然onmessage为注册
  winifrStorageProxy.onload = () => {
    // 发送存储指令
    winifrStorageProxy.contentWindow.postMessage(
      JSON.stringify(
        { 
          action: 'setItem', 
          key: 'showbo', 
          data: new Date().toLocaleString()
        }
    ), '*')
    // 发送读取指令
    winifrStorageProxy.contentWindow.postMessage(JSON.stringify({
      action: 'getItem',
      key: 'showbo'
    }), '*')
  }
</script>
```

## 8、async/await

## 9、深拷贝

- 递归
- 判断类型
- 检查环（循环引用）
- 忽略原型

```js
function deepClone(obj = {}) {
  if (typeof obj !== 'object' || obj === null) return obj
  let result
  if (obj instanceof Array) result = []
  else result = {}
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      result[key] = deepClone(obj[key])
    }
  }
  return result
}
```

## 10、使用正则实现trim方法

```js
function trim(str){
  return str.replace(/^\s+|\s+$/g, '')
}
```

## 11、继承

### 11.1 原型方法

```js
function People(name, age){
  this.name = name
  this.age = age
}
People.prototype.move = function(){
  console.log("我动了")
}
function User(sex, name, age){
  People.call(this, name, age)
  this.sex = sex
}
function temp(){}
temp.prototype = People.prototype
User.prototype = new temp()
User.prototype.say = function(){
  console.log('我说话了')
}
```

### 11.2 class方法

```js
class People{
    constructor(name, age){
        this.name = name        this.age = age    }
    say(){
        console.log(this.name + '今年' + this.age + '岁了')
    }
}
class User extends People {
    constructor(height, name, age){
        super(name, age)
        this.height = height    }
    jump(){
        console.log(this.name + "跳了一下")
    }
}
```

## 12、数组去重

### 12.1 计数排序变形

### 12.2 set去重

```js
function unique(arr){
  return Array.from(new Set(arr))
}
```

### 12.3 for循环使用splice去重
```js
function unique (arr){
  for (let i=0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j]) {
        arr.splice(j, 1)
        j--
      }
    }
  }
  return arr
}
```

### 12.4 new Set去重

```js
[...new Set(arr)]
```

### 12.5 Map

```js
function unique (arr) {
  const map = new Map()
  return arr.filter(value => !map.has(value) && map.set(value))
}
```

## 13、原型与原型链的理解

每个实例都有其__proto__，他对应其构造函数的prototype。
在js中，没有java中类的说法。js为了模拟new使用了原型来实现。
实例对象的__proto__ === 其构造函数的.prototype
每个实例出来的对象__proto__中的constructor都指向它的构造函数

```js
const demo1 = new Demo1()
demo1.__proto__ === Demo1.prototype
```

```js
demo1.__proto__.constructor === Demo1
```

### 13.1 __proto__ 与 prototype 的区别

- __proto__为隐式原型，prototype为显示原型。
- 每个对象都有他的__proto__指向其构造函数的原型对象。
- 每个方法除了有__proto__外，还会有prototype指向其方法的原型对象。

### 13.2 公式

xxx的原型===其构造函数的prototype
```js
对象.__proto__ === 其构造函数.prototype
```

所有构造函数都是由Function构造的
```js
构造函数.__proto__ === Function.prototype
```

Object.prototype原型是null
```js
Object.prototype.__proto__ === null
```

## 14、如何实现类

## ### 方法一：构造函数法

```js
function Cat() {
  this.name = '小猫'
}
let cat1 = new Cat()

console.log(cat1.name)
```

 > 类的属性和方法，可以定义在构造函数的prototype对象上

```js
Cat.prototype.say = function(){
  console.log('喵喵')
}
```

### 方法二：Object.create()法

```js
let Cat = {
  name: '小猫',
  say() {
    alert('喵喵')
  }
}
let cat1 = Object.create(Cat)

console.log(cat1.name)
cat1.say()
```

### 方法三：极简主义法

```js
let Animal = {
  createNew(){
    let animal = {}
    animal.sleep = ()=>alert('我是动物，我要睡觉')
    return animal
  }
}
let animal = Animal.createNew()

animal.sleep()
```

> 继承的话可以直接在内部调用前者的createNew即可

```js
let Cat = {
  createNew(){
    let cat = Animal.createNew()
    cat.name = '小猫咪'
    cat.age = '20岁'
    cat.say = ()=>alert('我是猫咪')
    return cat
  }
}
let cat = Cat.createNew()

console.log(cat.name, cat.age)
cat.say()
cat.sleep()
```

> 私有属性，只要将变量不挂载的对象上即可

```js
let Cat = {
  createNew(){
    let cat = {}
    let realName = '猫神' // 私有变量
    cat.name = '小猫'
    cat.sayRealName=()=>alert(realName)
    return cat
  }
}
let cat1 = Cat.createNew()

console.log(cat1.name)
cat1.sayRealName()
```

> 数据共享，只需要把变量声明在createNew外面即可

```js
let Cat = {
  shareData: '我被共享了',
  createNew(){
    let cat = {}
    cat.showShare=()=>alert(Cat.shareData)
    cat.changeShare=()=>Cat.shareData='共享数据被更改了'
    return cat
  }
}
let cat1 = Cat.createNew()
let cat2 = Cat.createNew()

cat1.showShare()
cat2.showShare()
cat1.changeShare()
cat2.showShare()
console.log(Cat.shareData)
```

## 15、Js基本类型和复杂类型

### 15.1 区别

1. 内存分配不同
    - 基本数据类型存储在栈中
    - 复杂数据类型存储在堆中，栈中存储的变量指向堆中的引用地址
2. 访问机制不同
    - 基本数据类型是按值访问
    - 复杂数据类型按引用访问
3. 赋值时不同
    - 基本数据类型：a=b是将b的值给a，a和b完全独立
    - 复杂数据类型：a=b将b保存的内存引用地址赋值给新的变量，a和b的地址是一样的，互相影响
4. 传参不同
    - 基本数据类型：把对应的值传递
    - 复杂数据类型：把内存引用地址传递

### 15.2 基本数据类型

- string
- number
- bigint
- boolean
- symbol
- undefined
- null

### 15.3 复杂数据类型

- Object
    - Array
    - Regexp
    - Data
    - Math
    - Function

## 16、数组方法

- pop() 弹出最后一个值，并返回出来
- push() 在最后面添加一个或多个值
- shift() 弹出第一个值，并返回出来
- unshift() 在最前面添加一个或多个值

### 16.1 改变原数组

pop、push、reverse、shift、sort、splice、unshift

### 16.2 不改变原数组

concat、join、slice

## 17、bind/call/apply的区别

1. 首先他们都是用来改变函数的this上下文。
2. bind是返回改变后的函数，call和apply是改变上下文后执行该函数。
3. bind和call第一个参数为需要改变上下文的对象，从第二个开始以参数列表显示。
4. apply第一个参数同样是改变上下文的对象，第二个是一个参数数组

### 简单实现一个call

```js
Function.prototype.myCall = function (context, ...args) {
  if (typeof this !== 'function') {
    console.error('not function')
  }
  context = context || window
  context.__fn = this
  return context.__fn(...args)
}
```

### 简单实现一个bind
```js
Function.prototype.bind2 = function(obj, ...args) {
  const that = this // 保留原本 this
  return function (...back) {
    // 修改原本 this 指向参数指向的 obj 对象，并将 bind 所带的参数保留下来
    that.call(obj, ...args, ...back)
  }
}

const a = {
  test: 2
}

function b () {
  // 获取 bind2 后面传的所有参数
  console.log(arguments) // output 1, 2, 3, 4
  // 判断当前 this 是否指向 a
  console.log(this === a) // output true
}

b.bind2(a, 1, 2, 3, 4)()
```

## 18、同步和异步

同步和异步是一种消息通知机制

- 同步阻塞：需要等待上一个代码执行完毕，下一个代码才可执行
- 异步非阻塞：在上一个代码执行后，不需要等待其返回结果。可以继续往后执行，回头在获得该结果。

## 19、回调的问题

回调就是函数指针的调用，可以简单理解为一个回头调用的函数。

回调容易造成回调地狱，造成代码维护性差。可以使用观察者模式、promise、async/await来处理。


## 20、如何中断ajax请求

使用 `XMLHttpRequest` 对象的 `abort` 方法中断ajax请求。

> 不能阻止向服务器发送请求，只能停止当前的 `ajax` 请求

```js
const xhr = new XMLHttpRequest()
postBtn.onclick = ()=>{
  xhr.open('get', '/api', true)
  xhr.onload = ()=>{
    console.log(xhr.requestText)
  }
  xhr.send()
}

stopBtn.onclick = ()=>{
  console.log('停止请求')
  xhr.abort()
}
```

## 21、模块化

CommonJs、AMD、CMD、ES6

- CommonJS是执行时编译
- ES6是静态编译

### 21.1 好处

- 避免命名冲突(减少命名空间污染)
- 更好的分离, 按需加载
- 更高复用性
- 高可维护性

## 22、简单实现EventHub

```js
/**
 * 保存事件
 * {
 *   payMoney: [
 *     ()=>console.log("花钱")
 *   ],
 *   getMoney: [
 *     ()=>console.log("爸爸拿钱"),
 *     ()=>console.log("儿子拿钱")
 *   ]
 * }
 */
let fnLists = {}

// 事件中心
let eventHub = {
  // 添加事件
  on(eventName, fn) {
    // 如果之前没添加，则新建一个事件数组
    if (!fnLists[eventName]) {
      fnLists[eventName] = []
    }
    // 往事件数组添加事件
    fnLists[eventName].push(fn)
  },
  // 触发事件
  trigger(eventName, data) {
    // 获取我们对应事件名的事件数组
    let fnList = fnLists[eventName]
    if (!fnList) return
    // 遍历该数组的所有事件
    for (let i = 0; i < fnList.length; i++) {
      fnList[i](data)
    }
  }
}
```

## 23、适配服务端和浏览器

```js
function getDefaultAdapter() {
  let adapter
  if(typeof XMLHttpRequest !== 'undefined') {
    adapter = '浏览器'
  }else if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]') {
    adapter = '服务端'
  }
  return adapter
}
```

## 24、那些操作会造成内存泄露

1. 闭包
2. 意外的全局变量
3. 定时器 setTimeout setInterval

```js
clearTimeout()
clearInterval()
```

4. 给 DOM 对象中添加属性是引用类型，如果 DOM 不销毁，则一直存在

```js
window.onunload = function(){
  属性 = null
}
```

## 25、如何延迟执行代码

1. script 标签添加 defer 属性，告诉浏览器立刻下载，但是延迟执行

```html
<script src='main.js' defer="defer"></script>
```

> 等执行性到 </html> 才执行该文件

2. script 标签添加 async 属性，异步加载。会在 load 事件前执行

```html
<script src="main.js" async></script>
```

3. 通过js，动态创建 script 标签修改 src 属性进行引入。
4. 使用 jquery 的 getScript

```js
$.getScript('文件路径', ()=>{
  // 文件加载成功，执行回调
})
```

5. setTimeout 延迟执行函数
6. 将 script 标签放置最后

## 26、排序算法

### 快速排序

```js
const arr = [1,23,2,9,0,21,228,83,-1]
const quickSort = (arr) => {
  if(arr.length <= 1) return arr
  const middle = arr.splice(Math.floor(arr.length / 2), 1)[0]
  const left = []
  const right = []
  for(let i = 0; i < arr.length; i++) {
    if(arr[i] > middle) right.push(arr[i])
    else left.push(arr[i])
  }
  return quickSort(left).concat(middle, quickSort(right))
}
```

## 27、纯函数

涉及到一些函数式编程的概念，指的是给定的输入，返回相同的输出的函数。  
好处：  
- 可以产生可测试的代码
- 可读性强
- 可以缓存，通过缓存

## JSONP原理

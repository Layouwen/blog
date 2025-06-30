---
title: 速览JavaScript字符串、数组、对象、MathAPI
date: 2021-03-28 02:44
tags:
  - 博客
  - 前端
  - javascript
categories:
  - 博客
  - 前端
---

## 1、字符串

### 1.1 获取字符串的对应下标数值

```js
const str = 'abc123ABC'
console.log(str[1])  // 输出结果为：b
console.log(str.chatAt(1))  // 输出结果为：b
```

### 1.2 获取字符串的长度

```js
const str = "123456"
console.log(str.length)  // 长度为：6
```

### 1.3 获取字符串的Unicode的编码

```js
const str = '中国'
console.log(str.charCodeAt(0))  // unicode编码为：20013
```

> 如果不指定下标，默认获取0

### 1.4 Unicode的编码转字符串

```js
const arr = [25105, 29233, 20320]
console.log(String.fromCharCode(arr[0], arr[1], arr[2]))  // 输出结果为：我爱你
```

### 1.5 查找是否有对应字符串

```js
const str = 'layouwen = layouwen'
console.log(str.indexOf('l'))  // 找到返回下标 0
console.log(str.indexOf('1', 4)  // 从下标 4 开始查找 返回下标 11
console.log(str.indexOf('s'))  // 找不到返回 -1
```

### 1.6 从后往前查找最后一个索引

```js
const str = 'hello word'
console.log(str.lastIndexOf('o'))  // 7
console.log(str.lastIndexOf('o', 8))  // 7
console.log(str.lastIndexOf('o', 5))  // 4
```

### 1.7 截取字符串

```js
const str = '12345678'
console.log(str.slice(0, -1))  // 从下标 0 开始，截取到倒数第一个字符串 之前 
// 1234567

console.log(str.subString(1, str.length - 1))  // 234567
```

### 1.8 分割字符串为数组

```js
const str = 'la-you-wen'
console.log(str.split())  // ['la-you-wen']
console.log(str.split('-'))  // ['la', 'you', 'wen']
console.log(str.split('-', 1) // 只分割一个 ['la']
```

### 1.9 连接字符串

```js
const str1 = 'la'
const str2 = 'you'
const str3 = 'wen'
console.log(str1.concat(str2, str3))  // layouwen
```

### 1.10 大小写转换

```js
const str1 = 'abc'
const str2 = 'DEF'
console.log(str1.toUpperCase())  // ABC
console.log(str2.toLowerCase())  // def
```

### 1.11 去除首位空格

```js
const str = '    123    4     '
console.log(str.trim())  // 123    4
```

## 2、数组

### 2.1 末尾添加

```js
const arr = [1, 2, 3]
console.log(arr.push(4, 5))  // 返回添加后数组的长度 结果为：5
console.log(arr)  // [1, 2, 3, 4, 5]
```

### 2.2 末尾删除

```js
const arr = [4, 6, 2]
console.log(arr.pop())  // 返回删除的值 结果为：2
console.log(arr)  // [4, 6]
```

### 2.3 开头添加

```js
const arr = [4, 2, 3]
console.log(arr.unshift(1))  // 返回添加后数组长度 结果为：4
console.log(arr)  // [1, 4, 2, 3]
```

### 2.4 开头删除

```js
const arr = [7, 5, 2]
console.log(arr.shift())  // 返回删除的值 结果为：7
console.log(arr)  // [5, 2]
```

###  2.5 添加、删除、替换

```js
const arr1 = [1, 2, 3]
arr1.splice(1, 0, 4, 5, 6)
console.log(arr1)  // [1, 4, 5, 6, 2, 3]

const arr2 = [1, 2, 3]
console.log(arr2.splice(1, 2))  // 从下标 1 开始，删除 2 个，并返回删除的内容 结果为：[2, 3]
console.log(arr2)  // [1]

const arr3 = [1, 2, 3, 4]
console.log(arr3.splice(1, 3, 'a', 'b', 'c'))  // 从下标 1 开始，删除 3 个，然后添加 a b c 结果为： [2, 3, 4]
console.log(arr3)  // [1, 'a', 'b', 'c']
```

### 2.6 排序

```js
const arr = [1, 2, 6, 3, 5, 9, 0]
arr.sort((a, b)=>a-b)  // a - b 升序  b - a 是降序
console.log(arr)
```

### 2.7 数组拼接

```js
const arr1 = [1, 2, 3]
const arr2 = [4, 5, 6]
const arr3 = ['a', 'b']
console.log(arr1.concat(arr2, arr3))  // 返回拼接后的数组
// [1, 2, 3, 4, 5, 6, "a", "b"]
console.log(arr1)  //  [1, 2, 3]
```

### 2.8 数组转字符串

```js
const arr = ['hello', 'word', '你好']
console.log(arr.join(','))  // 返回拼接后的字符串
// hello,word,你好
```

### 2.9 数组反转

```js
const arr = [1, 2, 3, 7, 6, 5, 4, 0]
console.log(arr.reverse())  // 返回颠倒后的数组
// [0, 4, 5, 6, 7, 3, 2, 1]
console.log(arr)  // 原数组也会被改变 [0, 4, 5, 6, 7, 3, 2, 1]
```

### 2.10 从前往后查找数组时候存在值

```js
const arr = ['a', 'b', 'c', 'a', 'd']
console.log(arr.indexOf('a', 3))  // 从下标 3 开始查找
// 3
console.log(arr.indexOf('a', -1))  // 从倒数第 1 个开始找
// -1
```

### 2.11 从后往前查找数组时候存在值

```js
const arr = ['a', 'b', 'c', 'a', 'd']
console.log(arr.lastIndexOf('a', 2))  // 从下标 2 开始向左查找
// 0
console.log(arr.lastIndexOf('a', -1))  // 从倒数第 1 个开始向左找
// 3
```

### 2.12 数组切割

```js
const arr = ['a', 'b', 'c', 'a', 'd']
console.log(arr.slice(1, 4))  // ['b', 'c', 'a']
console.log(arr.slice(-3, 1))  // []
console.log(arr.slice(-3, -1))  // ['c', 'a']
console.log(arr)  // ['a', 'b', 'c', 'a', 'd']
```

### 2.13 遍历数组

```js
const demoArr = ['a', 'b', 'c', 'a', 'd']
const thisArr = ['我是改变this', '的数组']
demoArr.forEach(function(value, index, arr){
  console.log(value, index, arr)
  console.log(this)
},thisArr)  // 改变回调中的 this 指向
demoArr.forEach((value, index, arr)=>{  // 箭头函数因为本身没有 this 所以第二个参数无效
  console.log(value, index, arr)
})
```

### 2.14 数组过滤

```js
const demoArr = ['a', 'b', 'c', 'a', 'd']
const thisArr = ['我是改变this', '的数组']
const newArr1 = demoArr.filter(function (value, index, arr) {
  console.log(value, index, arr)
  console.log(this)
  return value !== 'c' // 将所有为 true 的结果返回出去
}, thisArr) // 改变回调中的 this 指向
console.log('我是新数组：')
console.log(newArr1)  // ["a", "b", "a", "d"]
console.log('我是原数组：')
console.log(demoArr)  // ["a", "b", "c", "a", "d"]
const newArr2 = demoArr.filter((value, index, arr) => {
  // 箭头函数因为本身没有 this 所以第二个参数无效
  console.log(value, index, arr)
  return value === 'c'
})
console.log(newArr2)  // ['c']
```

### 2.15 数组加工

```js
const demoArr = ['a', 'b', 'c', 'a', 'd']
const thisArr = ['我是改变this', '的数组']
const newArr = demoArr.map((item, index, arr) => {
  console.log(item, index, arr)
  return (item += '嗯？') // 将结果添加到新的数组中，返回出去
})
console.log(newArr) // ["a嗯？", "b嗯？", "c嗯？", "a嗯？", "d嗯？"]
console.log(demoArr) // ["a", "b", "c", "a", "d"]
demoArr.map(function (item, index, arr) {
  console.log(this) // ['我是改变this', '的数组']
}, thisArr)
```

### 2.16 数据集合加工

```js
const demoArr = [10, 10, 10, 10]
const result = demoArr.reduce((sum, value, index) => { // sum 上一次的结果
  return sum + value
}, 1000) // 初始值，如果不传默认为数组第一位
console.log(result) // 1040
```

### 2.17 某一个为真返回真

```js
const falsyArr1 = [false, 1, 0, 0]
const falsyArr2 = [false, undefined, 0, 0]
const status1 = falsyArr1.some((value, index, arr) => {
  console.log(value, index, arr)
  return value // 只要有一个为 真值 则返回 true，否则返回 false
})
const status2 = falsyArr2.some(value => value)
console.log(status1, status2) // true false
```

### 2.18 所有为真才返回真

```js
const falsyArr1 = [1, 1, true, 0]
const falsyArr2 = ['a', true, 1, 1]
const status1 = falsyArr1.every((value, index, arr) => {
  console.log(value, index, arr)
  return value // 全部都为 真值 则返回 true，否则返回 false
})
const status2 = falsyArr2.every(value => value)
console.log(status1, status2) // false true
```

## 3、对象

### 3.1 获取所有key值

```js
const obj = {
  name: 'layouwen',
  age: '21'
}
console.log(Object.keys(obj)) // ["name", "age"]
```

### 3.2 获取所有value值

```js
const obj = {
  name: 'layouwen',
  age: '21'
}
console.log(Object.values(obj)) // ["layouwen", "21"]
```

### 3.3 删除某一项键值对

```js
const obj = {
  name: 'layouwen',
  age: '21'
}
delete obj.age // 连带键值对删除
console.log(obj) // {name: "layouwen"}
```

## 4、JSON

### 4.1 转JSON

```js
const obj = {
  name: 'layouwen',
  age: 21
}
console.log(JSON.stringify(obj)) // {"name":"layouwen","age":21}
```

### 4.2 转实例

```js
const objJSON = '{"name": "yqq", "age": 30}'
console.log(JSON.parse(objJSON)) // {name: "yqq", age: 30}
```

> 字符串中的 key 必须用 双引号

## 5、Math

### 5.1 圆周率

```js
console.log(Math.PI) // 3.141592653589793
```

### 5.2 取整

#### 5.2.1 向上取整

```js
console.log(Math.ceil(1.3)) // 2
```

#### 5.2.2 向下取整

```js
console.log(Math.floor(1.3)) // 1
```

#### 5.2.3 四舍五入

```js
console.log(Math.round(1.4)) // 1
console.log(Math.round(1.5)) // 2
```

### 5.3 随机数

```js
console.log(Math.random()) // 随机 0 - 1 之间的数字
```

### 5.4 最大值

```js
console.log(Math.max(5, -1, 9, 4)) // 9
const arr = [4, 3, 2, 8, 5]
console.log(Math.max(...arr)) // 8
```

### 5.5 最小值

```js
console.log(Math.min(5, -1, 9, 4)) // -1
const arr = [4, 3, 2, 8, 5]
console.log(Math.min(...arr)) // 2
```

### 5.6 绝对值

```js
console.log(Math.abs(-10))
// 10
```
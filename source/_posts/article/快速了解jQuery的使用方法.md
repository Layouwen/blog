---
title: 快速了解jQuery的使用方法
date: 2020-02-29 19:38
tags:
  - 博客
  - 前端
  - jquery
categories:
  - 博客
  - 前端
---

## 目录
+ 一、jQuery如何获取元素
+ 二、jQuery的链式操作是怎么样的
+ 三、jQuery如何创建元素
+ 四、jQuery如何移动元素
+ 五、jQuery如何修改元素

## 一、jQuery如何获取元素
有两种方法

第一种使用CSS选择表达式方式：



```javascript
// 选择整个文档的元素
$(document)	

// 选择ID为myID的网页元素
$('#myId')	

// 选择class为myClass的div元素
$('div.myClass') 

// 选择name属性等于first的input元素
$('input[name=first]')
```



第二种使用jQuery的表达式（[https://api.jquery.com/category/selectors/](https://api.jquery.com/category/selectors/)）:



```javascript
//选择网页中第一个a元素
$('a:first') 

//选择表格的奇数行
$('tr:odd') 

// 选择表单中的input元素
$('#myForm :input') 

//选择可见的div元素
$('div:visible') 

// 选择所有的div元素，除了前三个
$('div:gt(2)') 

// 选择当前处于动画状态的div元素
$('div:animated') 
```



## 二、<font style="color:#262626;">jQuery的链式操作是怎么样的</font>
<font style="color:#262626;">链式操作简单来说就是直接在前面语句后面接着使用 </font>`<font style="color:#262626;">.</font>` 来连接



```javascript
$('div').find('h3').eq(2).html('Hello')
```



上面这句话等于



```javascript
// 找到div元素
$('div') 

// 选择其中的h3元素
$.find('h3') 
// 选择第3个h3元素
$.eq(2)

// 将它的内容改为Hello
$.html('Hello') 
```



这么对比后是不是发现链式操作方便了很多



## 三、jQuery如何创建元素
在jQuery中创建元素只需要将html代码直接输入到参数内即可



```javascript
$('<div>111</div>')

$('<li class="new">new list item</li>')

$('ul').append('<li>list item</li>')
```



## 四、<font style="color:#262626;">jQuery如何移动元素</font>
<font style="color:#262626;">具体方法是 </font>

<font style="color:#262626;">选择元素+操作方法+目标元素</font>

具体操作方法有以下几种：



```javascript
在现存元素的外部，从后面插入元素
.insertAfter()和.after()

在现存元素的外部，从前面插入元素
.insertBefore()和.before()

在现存元素的内部，从后面插入元素
.appendTo()和.append()

在现存元素的内部，从前面插入元素
.prependTo()和.prepend()
```



例如：

将div元素移动到p元素后面



```javascript
$('div').insertAfter($('p'))
```



## <font style="color:#262626;">五、jQuery如何修改元素</font>
<font style="color:#262626;">获取元素的值</font>



```javascript
// html()不带参数为取值
$('h1').html()
```



向元素赋值



```javascript
// html()带参数为赋值
$('h1').html('Hello')
```



复制元素



```javascript
// 格式为 需要复制的元素+.clone().appendTo+目标位置	
$( ".hello" ).clone().appendTo( ".goodbye" )
```



删除元素



```javascript
// 直接在输入你要删除的元素节点+remove即可
$( ".hello" ).remove()
```




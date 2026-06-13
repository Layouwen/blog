---
title: CSS 篇
date: 2021-03-22T23:11:34.000Z
tags:
  - 汇总
  - 知识点总结
  - css
categories:
  - 汇总
  - 知识点总结
uuid: fd6e976c-d6c8-4c2b-9a58-033a7550274a
---

# 理论类
## 盒子模型

元素在页面的尺寸计算方式

content-box 是“内容宽度”: width/height 只是包含 content, 实际大小需要加 padding 和 border
border-box 是“最终显示宽度”: width/height 包含 content + padding + border, 最终显示宽高不会被撑大

## 垂直居中的方法

一般都是直接给父盒字加上一个 flex 直接设置它垂直居中和水平居中
简单给盒子上下同样的 padding 或 margin  
如果是定位的话可以给 left、top50%，使用 translate(-50%,-50%)  

## flex使用过吗

常用的属性

- flex-wrap
- flex-direction
- flex-grow: 1;
- flex-shrink: 1;
* flex-basis 和 width 区别
* shrink 压缩规则
* align-items vs align-content
* 主轴/交叉轴

## Grid

flex 适合一维布局, grid 适合二维布局多一点

wrapper 元素
- grid-template-columns: repeat(3, 1fr);
- grid-gap: 10px;
- grid-auto-rows: minmax(100px, auto);
子元素
- grid-column: 1 / 3;
- grid-row: 1

## BFC 是什么

块级格式化上下文（Block frmating context）。

BFC 是独立渲染区域，内部布局不会影响外部。

解决哪些问题：

1. 避免垂直 margin 合并
2. 避免垂直 margin 溢出
3. 自适应两栏布局
4. 防止高度塌陷

## 防止高度坍塌

**4** 种方案

1. 父元素添加 overflow: hidden
2. 在浮动元素后面添加元素，声明 clear: both
3. 伪元素:after
4. 父元素添加浮动

## CSS选择器优先级

以前流行个十百千计数法。  

- !important
* 行内样式
* id/class/tag
* 继承权重最低
* :not() 本身不增加权重
* :where() 权重为 0
* CSS Layers

## 清楚浮动

```css
.clearfix:after {
  content: '';
  display； block；
  clear: both;
}
```

## CSS3新特性

- 圆角（border-radius）
- 阴影（box-shadow）
- 过渡效果（transition）
- 翻转（transform）
- 动画（animation）
- 媒体查询（@media）
- 弹性盒子（flex）


## position 几种类型

relative 相对定位
absolute 绝对定位
fixed 固定定位
sticky 粘滞定位
static 默认

## rem和em的区别

em 继承父元素的字体大小，表示倍数
rem 继承\<html\>的字体大小，表示倍数

## 网页布局

1. 正常流布局
2. 弹性盒子布局
3. Grid布局
4. 浮动布局
5. 定位布局
6. 表格布局
7. 多列布局

## ::before 和 :after 中单冒号和双冒号的区别

CSS3 之后：
* 伪类使用单冒号 :
* 伪元素使用双冒号 ::
兼容 IE 才会都使用单冒号

## 隐藏元素的方法

display
- none 隐藏
- block 显示

visibility
- visible 显示
- hidden 隐藏

> display 不占位置, visibility 占位置

overflow
- hidden 溢出部分隐藏
- visible 显示
- auto 自动出现滚动条
- sroll 一直有滚动条

position

```css
left: -9999px
```

transform

```css
transform: translateX(-9999px);
transform: translateY(-9999px);
```

# 实操类

## flex布局题

### 1 如何实现flex最后一行靠左对齐

```html
<div class="container">
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
</div>
```

#### 1.1 如果每行列数是固定的

##### 方法一：模拟space-between和间隙

[参考链接](https://codepen.io/layouwen/pen/zYwevNK)

```css
.container {
    display: flex;
    flex-wrap: wrap;
}
.list {
    width: 24%; height: 100px;
    background-color: skyblue;
    margin-top: 15px;
}
.list:not(:nth-child(4n)) {
    margin-right: calc(4% / 3);
}
```

##### 方法二：根据个数最后一个元素动态匹配

```css
.container {
    display: flex;
    /* 两端对齐 */
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    width: 24%; height: 100px;
    background-color: skyblue;
    margin-top: 15px;
}
/* 如果最后一行是3个元素 */
.list:last-child:nth-child(4n - 1) {
    margin-right: calc(24% + 4% / 3);
}
/* 如果最后一行是2个元素 */
.list:last-child:nth-child(4n - 2) {
    margin-right: calc(48% + 8% / 3);
}
```

#### 1.2 每一子项宽度不固定

##### 方法一：最后一个元素margin-right: auto

```css
.container {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    background-color: skyblue;
    margin: 10px;
}
/* 最后一项margin-right:auto */
.list:last-child {
    margin-right: auto;
}
```

##### 方法二：创建伪元素并设置flex:auto或flex:1

```css
.container {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    background-color: skyblue;
    margin: 10px;
}
/* 使用伪元素辅助左对齐 */
.container::after {
    content: '';
    flex: auto;    /* 或者flex: 1 */
}
```

### 2 多列等高布局

实现方式就是通过padding-bottom给一个很大的值，接着使用margin-bottom给负数等大的值对冲实现。

## flex布局题

### 1 如何实现flex最后一行靠左对齐

```html
<div class="container">
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
    <div class="list"></div>
</div>
```

#### 1.1 如果每行列数是固定的

##### 方法一：模拟space-between和间隙

[参考链接](https://codepen.io/layouwen/pen/zYwevNK)

```css
.container {
    display: flex;
    flex-wrap: wrap;
}
.list {
    width: 24%; height: 100px;
    background-color: skyblue;
    margin-top: 15px;
}
.list:not(:nth-child(4n)) {
    margin-right: calc(4% / 3);
}
```

##### 方法二：根据个数最后一个元素动态匹配

```css
.container {
    display: flex;
    /* 两端对齐 */
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    width: 24%; height: 100px;
    background-color: skyblue;
    margin-top: 15px;
}
/* 如果最后一行是3个元素 */
.list:last-child:nth-child(4n - 1) {
    margin-right: calc(24% + 4% / 3);
}
/* 如果最后一行是2个元素 */
.list:last-child:nth-child(4n - 2) {
    margin-right: calc(48% + 8% / 3);
}
```

#### 1.2 每一子项宽度不固定

##### 方法一：最后一个元素margin-right: auto

```css
.container {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    background-color: skyblue;
    margin: 10px;
}
/* 最后一项margin-right:auto */
.list:last-child {
    margin-right: auto;
}
```

##### 方法二：创建伪元素并设置flex:auto或flex:1

```css
.container {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
.list {
    background-color: skyblue;
    margin: 10px;
}
/* 使用伪元素辅助左对齐 */
.container::after {
    content: '';
    flex: auto;    /* 或者flex: 1 */
}
```

### 多列等高布局

实现方式就是通过padding-bottom给一个很大的值，接着使用margin-bottom给负数等大的值对冲实现。

## 两栏布局

使用浮动
```css
.left {
  float: left;
  width: 100px;
}
.right {
  margin-left: 100px;
  width: auto;
}
```

使用浮动，并触发right的BFC
```css
.left {
  float: left;
  width: 100px;
}
.right {
  overflow: hidden;
}
```

使用flex
```css
.left {
  width: 100px;
}
.right {
  flex: 1;
}
```

使用绝对定位
```css
.wrapper {
  position: relative;
}
.left {
  position: absolute;
  width: 100px;
}
.right {
  margin-left: 100px;
}
```

使用绝对定位
```css
.wrapper {
  position: relative;
}
.left {
  width: 100px;
}
.right {
  position: absolute;
  left: 100px;
  top: 0;
  right: 0;
  bottom: 0;
}
```

## 15、三栏布局

使用绝对定位
```css
.wrapper { position: relative; }
.left { width: 100px; position: absolute; left: 0; top: 0; }
.middle { margin-left: 100px; margin-right: 200px; }
.right { position: absolute; width: 200px; top: 0; right: 0; }
```

使用flex
```css
.left { width: 100px; }
.middle { flex: 1; }
.right { width: 200px; }
```

使用浮动
```css
.left { float: left; }
.middle { margin-left: 100px; margin-right: 200px; }
.right { float: right; } 
```

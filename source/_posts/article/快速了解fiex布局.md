---
title: 快速了解fiex布局
date: 2020-12-25 18:15
tags:
  - 博客
  - 前端
  - flex
  - css
categories:
  - 博客
  - 前端
---

## 1、如何让元素使用flex布局

给元素添加 flex 或 inline-flex 样式

- flex 在宽度不够使，直接换行显示
- inline-flex 在宽度不够时，显示一半，剩下的下一行接着显示。

```css
div {
    display: flex; /* inline-flex */
}
```

当元素添加 flex 后，添加 flex 的元素称为 容器，我们可以通过添加属性控制其内部子元素的排列方式。

## 2、改变元素流动方向

给 flex 容器，添加 flex-direction 样式

- row 从左往右排列
- row-reverse 从右往左排列
- colum 从上往下排列
- colum-reverse 从下往上排

```css
div {
    display: flex; 
    flex-direction: row; /* row-reverse colum colum-reverse */
}
```

## 3、元素是否换行

flex 的子元素是默认不会换行，当一行的子元素过多时，他们会自适应的相互挤压。
给 flex 容器，添加 flex-wrap 样式，即可控制是否换行

- no-wrap 不换行（默认）
- wrap 换行
- wrap-reverse 倒叙换行（简单来说就是与wrap的顺序反了过来）

```css
div {
    display: flex; 
    flex-wrap: wrap; /* no-wrap wrap-reverse */
}
```

## 4、元素水平对齐元素

给 flex 容器添加 justify-content 样式

- flex-start 元素和容器的左端对齐
- flex-end 元素和容器的右端对齐
- center 元素在容器里居中
- space-between 元素之间保持相等的距离
- space-around 元素周围保持相等的距离
- space-evenly 在 around 基础上，全部空隙自适应

```css
div {
    display: flex; 
    justify-content: space-between; /* flex-start flex-end center space-around space-evenly */
}
```

## 5、元素纵轴的分布方式

给 flex 容器添加 align-items 样式

- flex-start 顶端对齐
- flex-end 底端对齐
- center 居中对齐
- stretch 铺满
- baseline 基线对齐（用不到，可以不用管）

```css
div {
    display: flex; 
    align-items: flex-start; /* flex-end center stretch baseline */
}
```

## 6、多行布局

上面都是默认当行的情况下，现在是多行布局
给 flex 容器添加 align-content 样式

- flex-start 全部挤在顶端
- flex-end 全部挤在底端
- center 全部挤在中间
- stretch 全部平均的铺满
- space-between 分别上中下对齐
- space-around 在上中下对齐的基础上，全部间隙相同

```css
div {
    display: flex; 
    align-content: stretch; /* flex-start flex-end center stretch space-between space-around */
}
```

## 7、改变子元素排列的顺序

给 flex 容器内的，子元素盒子添加 order 样式
数字越小越靠前，越大越靠后

```css
div {
    display: flex; 
    order: -11; /* 负数 0 正数 */
}
```

## 8、控制子元素所占比

给 flex 容器内的，子元素添加 flex-grow 样式
用数字表示每个子元素所在大小

例如：两边盒子各占4分之1，中间盒子占4分之2

```css
child1 {
    flex-frow: 1;
}
child2 {
    flex-frow: 2;
}
child3 {
    flex-frow: 1;
}
```

## 9、固定子元素所占比

给 flex 容器内的，子元素添加 flex-shrink 样式

```css
div {
    flex-shrink: 0;
}
```
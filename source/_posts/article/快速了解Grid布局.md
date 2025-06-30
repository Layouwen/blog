---
title: 快速了解Grid布局
date: 2020-12-25 18:24
tags:
  - 博客
  - 前端
  - grid
  - css
categories:
  - 博客
  - 前端
---

## 1、如何让元素使用grid布局

给元素添加 grid 或 inline-grid 样式

## 2、设置行和列

使用 grid-template-columns 设置每一列的宽度
使用 gird-template-rows 设置每一行的高度

```css
div {
    grid-template-columns: 40px 50px auto 50px 40px;
    grid-template-rows: 25% 100px auto;
}
```

> 注意：属性可以使用 repeat函数 进行书写。
> 如：分为8列，每列百分之12.5。repeat(8, 12.5%)

## 3、跨列

使用 grid-column-start 设置起点
使用 grid-coulmn-end 设置终点

```css
div {
    grid-column-start: 2;
    grid-column-end: 4;
}
```

> 注意：当为负数的时候，从后面开始往前数。最后一格 -1，倒数第二格 -2。


## 4、设置网格的大小比

使用 span 给属性设置 跨域 的宽度

```css
div {
    grid-column-start: 2;
    grid-column-end: span 2;
}
```

## 5、跨列的缩写

使用 grid-column 可以对 start 和 end 进行缩写

```css
div {
    grid-column: 2 / 4; /* 从第 2 列开始，至第 4 列结束 */
}
```

## 6、跨行

使用 grid-row-start 设置起点
使用 grid-row-end 设置终点

```css
div {
    grid-row-start: 2;
    grid-row-end: 4;
}
```

> 注意：当为负数的时候，从后面开始往前数。最后一格 -1，倒数第二格 -2。

## 7、跨行的缩写

使用 grid-row 可以对 start 和 end 进行缩写

```css
div {
    grid-row: 2 / 4; /* 从第 2 行开始，至第 4 行结束 */
}
```

## 8、跨行跨列合并缩写

使用 grid-area 可以对 行 和 列 的 开始 和 结尾 进行设置
grid-area: 行的开始 列的开始 行的结束 列的结束;

```css
div {
    grid-area: 1 / 2 / 4 / -1;
}
```

## 9、顺序

当网格中没有设置跨行跨列的属性时，网格默认按出现位置排列。可以使用 order 进行调整。

- 正数 在0的后面，从小到大排列
- 0 默认
- 负数 在0的前面，从小到大排列

```css
div {
    order: -1;
}
```

## 10、设置列显示百分比

在开始的 grid-template-columns 设置后，再次修改属性，添加百分比

```css
div {
    gird-template-columns: 50%;
}
```

> 注意：还可以使用 fr 单位，平均分配
> grid-template-columns: 1fr 3fr; 第一列占宽度的1/4，第二列占宽度的3/4

## 11、设置行显示百分比

在开始的 grid-template-rows 设置后，在进行修改属性，添加百分比

```css
div {
    gird-template-rows: 50%;
}
```

> 注意：还可以使用 fr 单位，平均分配
> grid-template-rows: 1fr 3fr; 第一行占宽度的1/4，第二行占宽度的3/4

## 12、列和行的显示百分比合并缩写

grid-template 行显示 / 列显示

```css
div {
    gird-template: 1fr 3fr / 1fr;
}
```
---
title: dayjs计算连续打卡时间
date: 2022-02-06 01:38
tags:
  - 博客
  - 后端
  - nodejs
categories:
  - 博客
  - 后端
  - nodejs
---

## 前言

最近做的个人项目中，需要统计用户连续打卡的时间。网上搜索了很多资料，方法有很多。比如使用mysql分组条件查询、窗口排序等。考虑到自己需求没有这么复杂，只需要计算最近一次的连续天数。就是直接使用dayjs计算。

## 思路

1. mysql查询对应用户所有的打卡记录，进行降序排序。
2. 获取第一个时间，判断时候在今天或昨天的范围。
3. 如果是则表示仍在连续打卡范围，如果不是直接返回0天
4. 判断最后一次打开是否是今天，不是的话从昨天开始查找
5. 每次遍历天数加一，直到发现不是连续的则中断遍历

> 这里数据添加的时候，限制了一天只能打卡一次。所以记录中没有重复天数的。如果返回的数据有重复的，会导致最后的连续天数有误差。可以考虑进行去重。

## 实现

```ts
const getContinueDay = (list: { checkInTime: string }[]) => {
  let day = 0;
  if (!list.length) return day;
  let nowStartDay = dayjs().startOf('day');
  const left = dayjs().subtract(1, 'day').startOf('day');
  const right = dayjs().endOf('day');
  if (dayjs(list[0].checkInTime).isBetween(left, right, null, '[]')) {
    if (!dayjs(list[0].checkInTime).isSame(nowStartDay, 'day')) {
      nowStartDay = nowStartDay.subtract(1, 'day');
    }
    for (const i of list) {
      const dayStart = dayjs(nowStartDay).startOf('day');
      const dayEnd = dayjs(nowStartDay).endOf('day');
      if (!dayjs(i.checkInTime).isBetween(dayStart, dayEnd, 'day', '[]')) break;
      day++;
      nowStartDay = nowStartDay.subtract(1, 'day');
    }
  }
  return day;
};
```

## 效果

### 数据库

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/38fcde674535402693b7f1d9a7c82909~tplv-k3u1fbpfcp-zoom-1.image)

### 结果

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de9ef709e52748c384d0ec00e0746102~tplv-k3u1fbpfcp-zoom-1.image)
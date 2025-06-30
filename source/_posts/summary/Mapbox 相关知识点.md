---
title: Mapbox 相关知识点
date: 2023-03-22 23:11:34
tags:
  - 汇总
  - mapbox
categories:
  - 汇总
---

# 1. 表达式设置属性默认值

```json
paint: {
  "text-color": ["coalesce", ["get", "text-color"], "#348238"],
}
```

> 如果存在 `text-color` 的值则使用 `text-color`，否则取 `#348238`

# 2. easyTo 平滑移动页面

```ts
mapSDK.map.easeTo({
  padding: {
    right: 1000, // 右边留 1000px 距离
  },
  duration: 1000, // 持续时间
});
```

# 3. flyTo 监听结束状态

## 方式 1: 通过 easing 回调

```ts
mapSDK.map.flyTo({
  center: xxx,
  easing: t => {
    if (t === 1) console.log('到达终点, 动画结束')
    return t
  }
})
```

## 方式 2: 通过自定义事件数据

```ts
mapSDK.map.on('moveend', (e) => {
  if (e.moveend === 'flyend') {
	console.log('moveend')
  }
})

mapSDK.on('click', (e: any) => {
	mapSDK.map.flyTo({
	  center: e.coordinates,
	}, { moveend: 'flyend' });
}
```
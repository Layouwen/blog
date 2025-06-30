---
title: Vue中的.sync修饰符的理解
date: 2020-03-22 16:28
tags:
  - 博客
  - 前端
  - vue
categories:
  - 博客
  - 前端
---

## 案例

准备2个文件：Main.vue、Child.vue

### - 代码

Main.vue

```vue
<template>
  <div class="border">
    总金额：{{total}} 元
    <hr>
    <Child :money="total" v-on:update:money="total = $event"/>
  </div>
</template>

<script>
import Child from "./Child.vue";
export default {
  data() {
    return { total: 10000 };
  },
  components: { Child: Child }
};
</script>

<style>
.border {
  border: 3px solid black;
}
</style>
```

Child.vue

```vue
<template>
  <div class="child">
    <button @click="$emit('update:money', money-100)">
      <span>花钱</span>
    </button>
  </div>
</template>

<script>
export default {
  props: ["money"]
};
</script>


<style>
.child {
  border: 3px solid pink;
}
</style>
```

### 解释

Main.vue用外部表示

Child.vue用内部表示

`外部` 将total的值，赋值给money，然后 `内部` 通过props接收了money的值。当button触发click时，会使用$emit将money-100发送给 `外部` 。接着 `外部` 通过v-on接收了数据，并将money-100的结果，赋值给total。然后将total的值显示在div标签中。从而实现对数据的修改。

### .sync修饰符的出现

尤雨溪发现这种操作挺常用的，就为这种行为进行了一个简化。

将 `<Child :money="total" v-on:update:money="total = $event"/>` 简化为 `<Child :money.sync="total">` 通过使用.sync修饰符来实现上面一整局话的效果。

Main.vue

```vue
<template>
  <div class="border">
    总金额：{{total}} 元
    <hr>
    <Child :money.sync="total"/>
  </div>
</template>

<script>
import Child from "./Child.vue";
export default {
  data() {
    return { total: 5000 };
  },
  components: { Child: Child }
};
</script>

<style>
.border {
  border: 3px solid black;
}
</style>
```



.sync简单来说就是，监听是否有人修改本地的数据，如果有的话就对本地数据进行一个修改。


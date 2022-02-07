---
title: Vue组件刷新
date: 2020-11-02 20:50:59
tags: [vuejs]
---

在特定情况下，如果需要执行刷新操作，有四种可选方案：

1. 借助`router.go(0)`方法，刷新整个页面
2. 使用`forceUpdate()`方法，刷新当前组件
3. 使用`v-if`标记，刷新子组件
4. 使用`:key`属性，通过改变`key`的值来达到刷新子组件的目的

<!--more-->

## `forceUpdate`

```html
<template>
  <div id="app">
    <button @click="refresh">Refresh</button>
  </div>
</template>

<script lang="ts">
export default {
  methods: {
    refresh () {
      this.$forceUpdate();  // 刷新当前组件
      // this.$router.go(0);  // 刷新整个页面
    }
  }
};
</script>
```

## `v-if`

```html
<template>
  <div id="app">
    <button @click="refresh">Refresh</button>
    <SubComponent v-if="flag">This is SubComponent!</SubComponent>
  </div>
</template>

<script lang="ts">
export default {
  data: () => ({
    flag: true
  }),
  methods: {
    refresh () {
      this.flag = false;
      this.$nextTick(() => {
          this.flag = true;
      });
    }
  }
};
</script>
```

## `:key`

```html
<template>
  <div id="app">
    <button @click="refresh">Refresh</button>
    <SubComponent :key="key">This is SubComponent!</SubComponent>
  </div>
</template>

<script lang="ts">
export default {
  data: () => ({
    key: Math.random()
  }),
  methods: {
    refresh () {
      this.key = Math.random();
    }
  }
};
</script>
```

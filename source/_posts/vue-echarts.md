---
title: 在Vuejs中使用ECharts
date: 2020-11-14 20:45:37
tags: [vuejs, echarts]
---

## 安装

```bash
yarn add echarts @types/echarts
```

## ECharts.vue文件

```html
<template>
  <div style="width: 400px; height: 300px;"></div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator'
import echarts from 'echarts'

@Component
export default class ECharts extends Vue {
  @Prop({ default: () => ({}) }) private options!: echarts.EChartOption | echarts.EChartsResponsiveOption
  private _chart?: echarts.ECharts

  protected mounted () {
    this.draw()
  }

  @Watch('options', { deep: true })
  private draw () {
    if (!this._chart) {
      this._chart = echarts.init(this.$el as HTMLDivElement)
    }
    this._chart.setOption(this.options)
  }
}
</script>
```

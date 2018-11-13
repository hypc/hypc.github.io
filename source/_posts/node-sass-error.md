---
title: 【BUG】node-sass安装失败解决办法
date: 2018-07-15 21:22:34
tags: [npm, nodejs]
---

## 方案1

使用taobao镜像源

```bash
SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/ npm install node-sass
```

## 方案2

使用cnpm

```bash
cnpm install node-sass
```

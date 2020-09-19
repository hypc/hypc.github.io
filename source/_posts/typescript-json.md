---
title: 在typescript中引入json文件
date: 2020-09-15 20:39:17
tags: [typescript]
---

直接在typescript中导入json会抛出如下异常：

```
TS2732: Cannot find module ‘***.json'. Consider using '--resolveJsonModule' to import module with '.json' extension
```

要避免产生这个异常，你需要在`tsconfig.json`中配置如下两个属性：

```json
"resolveJsonModule": true,
"esModuleInterop": true,
```

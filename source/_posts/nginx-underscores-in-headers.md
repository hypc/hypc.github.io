---
title: Nginx：带下划线的请求头
date: 2018-09-26 08:57:57
tags: [nginx]
---

```txt
Syntax:  underscores_in_headers on | off;
Default: underscores_in_headers off;
Context: http, server
```

启用或禁用带下划线的请求头。当禁用下划线时，名称中包含下划线的请求头将被置为无效。

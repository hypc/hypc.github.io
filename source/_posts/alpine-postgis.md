---
title: 在Alpine中连接Postgis服务
date: 2020-07-28 21:45:47
tags: [docker, alpine, postgresql]
---

安装以下依赖：

```dockerfile
RUN apk add —-no-cache postgresql-dev gdal-dev geos-dev
```

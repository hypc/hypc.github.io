---
title: alpine设置时区
date: 2018-09-19 10:38:21
tags: [docker, alpine]
---

```dockerfile
RUN apk update && apk add ca-certificates \
    && apk add tzdata \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone
```

---
title: 在python:3.5-alpine镜像中安装psycopg2
date: 2018-07-15 21:29:02
tags: [docker, python]
---

需要先安装依赖包`postgresql-dev`、`gcc`、`python3-dev`、`musl-dev`：

```bash
apk update
apk add postgresql-dev gcc python3-dev musl-dev
pip install psycopg2
```

当然也可以先做一个基础镜像：

```dockerfile
FROM python:3.5-alpine

RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev

CMD ["python3"]
```

在国内也可以使用aliyun的镜像源：

```dockerfile
FROM python:3.5-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update \
    && apk add postgresql-dev gcc python3-dev musl-dev

CMD ["python3"]
```

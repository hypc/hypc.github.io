---
title: Deploying Vue Project in Docker
date: 2018-07-28 09:07:45
tags: [docker, vuejs]
---

## 准备工作

本文讲述怎么在Docker中部署[vuejs][]项目，该项目使用[vue-cli][]创建项目。

1. 安装docker服务，参考[Install Docker][]；
2. 安装docker-compose工具，参考[Install Docker Compose][]；
3. 拉取镜像node:6-alpine。

## 创建vue项目

```bash
npm install -g vue-cli
vue init webpack myproject
```

## 部署vuejs项目

### 编写`Dockerfile`文件

```dockerfile
FROM node:6-alpine

ADD . /app
WORKDIR /app

RUN npm install -g http-server \
    && npm install \
    && ng build \
    && cp dist/index.html dist/404.html

EXPOSE 80
CMD ["http-server", "/app/dist", "-p", "80"]
```

<!--more-->

### 编写`docker-compose.yaml`文件

```yaml
version: "2"
services:
    master:
        build: .
        restart: always
        ports:
            - 8080:80
```

### 运行

```bash
docker-compose up -d --build
```


[Docker]: https://www.docker.com
[vuejs]: https://vuejs.org/
[vue-cli]: https://github.com/vuejs/vue-cli
[Install Docker]: https://docs.docker.com/engine/installation/
[Install Docker Compose]: https://docs.docker.com/compose/install/
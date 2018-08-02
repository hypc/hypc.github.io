---
title: Deploying Angular2 Project in Docker
date: 2018-07-28 09:08:59
tags: [docker, angular]
---

## 准备工作

本文主要讲解怎么在Docker中部署[Angular2][]项目，当然，该Angular2项目是由[angular-cli][]创建的项目。

1. 安装`docker`服务，参考[Install Docker][]；
2. 安装`docker-compose`工具，参考[Install Docker Compose][]；
3. 拉取镜像`node:6-alpine`。

## 部署Angular2项目

### 编写`Dockerfile`文件

```dockerfile
FROM node:6-alpine

ADD . /app
WORKDIR /app

RUN npm install -g @angular/cli http-server \
    && npm install \
    && ng build -prod \
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

### 编译并运行项目

```bash
docker-compose up -d --build
```


[Angular2]: https://angular.io/
[angular-cli]: https://github.com/angular/angular-cli
[Install Docker]: https://docs.docker.com/engine/installation/
[Install Docker Compose]: https://docs.docker.com/compose/install/

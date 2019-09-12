---
title: 使用Docker部署Angular、React、Vue项目
date: 2019-09-11 14:55:54
tags: [docker, angular, reactjs, vuejs]
---

今天重新整理了一下[Angular][]、[React][]、[Vue][]的部署方案，采用docker+nginx方式进行部署。

* `nginx.conf`
* `Dockerfile`
* `docker-compose.yaml`

[Angular]: https://angular.io/
[React]: https://reactjs.org/
[Vue]: http://vuejs.org/

<!--more-->

## 编写配置文件

### 编写`nginx.conf`文件

```nginx
user  nginx;
worker_processes  auto;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile        on;
    keepalive_timeout  65;
    server {
        listen      80;
        server_name _;
        root        /usr/share/nginx/html;
        index       index.html index.htm;
        try_files   $uri $uri/ /index.html;
    }
}
```

### 编写`Dockerfile`文件

```dockerfile
### build app
FROM node:12.10-alpine as build-app

COPY . /app
WORKDIR /app

RUN npm install && npm run build

### final image
FROM nginx:alpine

COPY nginx.conf /etc/nginx/nginx.conf
COPY --from=build-app /app/dist /usr/share/nginx/html
```

### 编写`docker-compose.yaml`文件

```yaml
version: '3'
services:
  app:
    build: .
    image: app
    container_name: app
    ports:
      - 8080:80
    restart: always
```

## 启动服务

```bash
$ docker-compose up -d --build
```

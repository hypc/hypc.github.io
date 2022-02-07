---
title: 优化Docker镜像
date: 2020-12-30 15:14:50
tags: [docker]
---

* 选用更小的基础镜像：例如`alpine`或基于`alpine`构建的镜像

* 合并指令，减少镜像层级：将安装依赖、编译、移除源代码等指令合并成一条语句

* 去除不需要的依赖、库：删除用不到的依赖、库等，尽量精简镜像

* 分阶段构建镜像：分离编译镜像和部署镜像，下面是一个`nodejs`项目的示例：

    ```dockerfile
    FROM node:14-alpine as build

    ADD . /app
    RUN yarn install && yarn build

    FROM nginx:alpine

    COPY --from=build-app /app/dist /usr/share/nginx/html
    ```

大家可以在[DockerHub](https://hub.docker.com/)了解更多优秀软件的`Dockerfile`的写法。

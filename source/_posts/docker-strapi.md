---
title: 使用Docker部署Strapi服务
date: 2021-03-28 11:04:53
tags: [docker, cms]
---

`Headless CMS`是一个内容管理软件，它允许作者创建和管理内容，以及提供结构化数据给开发者，让开发者能够将数据展示在网站或者应用前端的一个独立系统中。

一个传统的，完整的`CMS`是同时负责后端的内容管理以及提供内容给最终用户。
但相比之下，一个`headless CMS`将前端分离出来，让开发者能够用最好的技术来建立优越的用户体验。

[Strapi][]是一款开源的、也是最受欢迎的`Headless CMS`。

`Strapi`这个名字取自`bootstrap`的后缀`strap`，然后因为它是一个提供快速生成安全可靠的`api`架构，
然后再加了一个`i`，合并就是`strapi`，`bootstrap`的有启动的意思，你可以用`strapi`来快速构建你的后端，可以快速让自己的项目启动。

[Strapi]: https://strapi.io/

```yaml
version: '3.7'

services:
  shaarli:
    image: shaarli/shaarli
    restart: always
    labels:
      - traefik.http.routers.shaarli.rule=Host(`shaarli.yourdomain.com`)
      - traefik.http.routers.shaarli.entrypoints=websecure
      - traefik.http.routers.shaarli.service=shaarli
      - traefik.http.services.shaarli.loadbalancer.server.port=80
    volumes:
      - data:/var/www/shaarli/data
      - cache:/var/www/shaarli/cache

volumes:
  data:
  cache:
```

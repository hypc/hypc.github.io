---
title: 使用Docker部署Wallabag服务
date: 2021-01-21 19:02:13
tags: [docker]
---

[Wallabag][]是一款稍后阅读工具。

[Wallabag]: https://github.com/wallabag/wallabag

```yaml
version: '3.7'

services:
  wallabag:
    image: wallabag/wallabag
    restart: always
    labels:
      - traefik.http.routers.wallabag.rule=Host(`wallabag.domain.com`)
      - traefik.http.routers.wallabag.entrypoints=web
      - traefik.http.routers.wallabag.service=wallabag
      - traefik.http.services.wallabag.loadbalancer.server.port=80
    environment:
      - SYMFONY__ENV__DOMAIN_NAME=http://wallabag.domain.com
    volumes:
      - data:/var/www/wallabag/data
      - images:/var/www/wallabag/web/assets/images

volumes:
  data:
  images:
```

---
title: 使用Docker部署Matomo系统
date: 2021-05-19 18:24:13
tags: [docker, matomo]
---

[Matomo][]（之前叫Piwik），是一款开源的网站流量统计系统。

[Matomo]: https://matomo.org/

```yaml
version: '3.7'

services:
  matomo:
    image: matomo
    restart: always
    labels:
      - traefik.http.routers.matomo.rule=Host(`matomo.yourdomain.com`)
      - traefik.http.routers.matomo.entrypoints=websecure
      - traefik.http.routers.matomo.service=matomo
      - traefik.http.services.matomo.loadbalancer.server.port=80
    environment:
      - MATOMO_DATABASE_HOST=matomo-db
      - MATOMO_DATABASE_USERNAME=root
      - MATOMO_DATABASE_PASSWORD=E315AECBADFF41E38C57EEFD9E03FCBC
      - MATOMO_DATABASE_DBNAME=matomo
    volumes:
      - html:/var/www/html
  matomo-db:
    image: mysql:5.7
    restart: always
    labels:
      - traefik.enable=false
    environment:
      - MYSQL_ROOT_PASSWORD=E315AECBADFF41E38C57EEFD9E03FCBC
    volumes:
      - data:/var/lib/mysql

volumes:
  html:
  data:
```

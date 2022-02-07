---
title: 使用Docker部署Wikijs系统
date: 2021-05-19 19:31:31
tags: [docker, wikijs]
---

[Wikijs][]是一款开源的基于nodejs的轻量级wiki系统。

[Wikijs]: https://js.wiki/

```yaml
version: '3.7'

services:
  wiki:
    image: requarks/wiki
    restart: always
    labels:
      - traefik.http.routers.wiki.rule=Host(`wiki.yourdomain.com`)
      - traefik.http.routers.wiki.entrypoints=websecure
      - traefik.http.routers.wiki.service=wiki
      - traefik.http.services.wiki.loadbalancer.server.port=3000
    environment:
      - DB_TYPE=postgres
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USER=postgres
      - DB_PASS=postgres
      - DB_NAME=wiki
    volumes:
      - content:/wiki/data/content

volumes:
  content:
```

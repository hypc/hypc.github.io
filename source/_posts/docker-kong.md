---
title: 使用Docker部署Kong服务
date: 2021-01-14 19:24:15
tags: [docker, kong]
---

[Kong][]是全球最受欢迎的开源API网关。专为多云和混合而建，针对微服务和分布式架构进行了优化。
[KongA][]是一款开源的[Kong][] UI管理工具。

[Kong]: https://konghq.com/
[KongA]: https://github.com/pantsel/konga

```yaml
version: '3.7'

services:
  kong:
    image: kong
    restart: always
    ports:
      - 8000:8000
    environment:
      - KONG_DATABASE=postgres
      - KONG_PG_HOST=postgres
      - KONG_CASSANDRA_CONTACT_POINTS=postgres
      - KONG_PROXY_ACCESS_LOG=/dev/stdout
      - KONG_ADMIN_ACCESS_LOG=/dev/stdout
      - KONG_PROXY_ERROR_LOG=/dev/stderr
      - KONG_ADMIN_ERROR_LOG=/dev/stderr
      - KONG_ADMIN_LISTEN=0.0.0.0:8001
  kong-migrate:
    image: kong
    restart: on-failure
    command: kong migrations bootstrap
    environment:
      - KONG_DATABASE=postgres
      - KONG_PG_HOST=postgres
      - KONG_CASSANDRA_CONTACT_POINTS=postgres
  konga:
    image: pantsel/konga
    restart: always
    ports:
      - 1337:1337
    environment:
      - NODE_ENV=production
    volumes:
      - kongadata:/app/kongadata
  postgres:
    image: postgres:12
    restart: always
    environment:
      - POSTGRES_USER=kong
      - POSTGRES_DB=kong

volumes:
  kongadata:
```

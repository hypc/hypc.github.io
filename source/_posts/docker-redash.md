---
title: 使用Docker快速搭建Redash服务
date: 2020-01-14 09:41:32
tags: [docker, redash]
---

[Redash][]是一款开源的BI工具，提供了基于web的数据库查询和数据可视化功能。

[Redash]: https://redash.io/

<video width="720" autoplay loop muted>
    <source src="/images/redash-intro-720.mp4" type="video/mp4">
</video>

<!--more-->

## 快速启动

**配置`docker-compose.yaml`文件**

```yaml
version: '2'
x-redash-service: &redash-service
  image: redash/redash
  depends_on:
    - postgres
    - redis
  env_file: ./env
  restart: always
services:
  redash:
    <<: *redash-service
    command: server
    ports:
      - 5000:5000
    environment:
      REDASH_WEB_WORKERS: 4
  scheduler:
    <<: *redash-service
    command: scheduler
    environment:
      QUEUES: "celery"
      WORKERS_COUNT: 1
  scheduled_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "scheduled_queries,schemas"
      WORKERS_COUNT: 1
  adhoc_worker:
    <<: *redash-service
    command: worker
    environment:
      QUEUES: "queries"
      WORKERS_COUNT: 2
  postgres:
    image: postgres:9.6-alpine
    env_file: ./env
    volumes:
      - ./postgres-data:/var/lib/postgresql/data
    restart: always
  redis:
    image: redis
    restart: always
```

**配置`env`环境变量文件**

```ini
PYTHONUNBUFFERED=0
REDASH_LOG_LEVEL=INFO
REDASH_REDIS_URL=redis://redis:6379/0
POSTGRES_PASSWORD=${postgres_password}
REDASH_COOKIE_SECRET=${redash_cookie_secret}
REDASH_SECRET_KEY=${redash_secret_key}
REDASH_DATABASE_URL=postgresql://postgres:${postgres_password}@postgres/postgres
```

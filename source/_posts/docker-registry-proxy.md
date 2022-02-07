---
title: 将docker registry设置为代理
date: 2021-06-30 16:38:28
tags: [docker]
---

{% tabs %}
<!-- tab gcr.io.yml -->
```yaml
version: 0.1
log:
  fields:
    service: registry
storage:
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :5000
  headers:
    X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
proxy:
  remoteurl: https://gcr.io
```
<!-- endtab -->

<!-- tab docker-compose.yml -->
```yaml
version: '3.7'

services:
  gcr_io:
    image: registry
    restart: always
    labels:
      - traefik.http.routers.gcr_io.rule=Host(`gcr.mirrors.yourdomain.com`)
      - traefik.http.routers.gcr_io.entrypoints=websecure
      - traefik.http.routers.gcr_io.service=gcr_io
      - traefik.http.services.gcr_io.loadbalancer.server.port=5000
    volumes:
      - ./gcr.io.yml:/etc/docker/registry/config.yml
```
<!-- endtab -->
{% endtabs %}

使用`docker-compose up -d`命令启动代理服务。


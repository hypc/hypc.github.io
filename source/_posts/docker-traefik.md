---
title: 使用Docker部署Traefik服务
date: 2021-01-12 18:56:16
tags: [docker, traefik]
---

[Traefɪk][]是一个为了让部署微服务更加便捷而诞生的现代HTTP反向代理、负载均衡工具。
它支持多种后台（Docker, Swarm, Kubernetes, Marathon, Mesos, Consul, Etcd,
Zookeeper, BoltDB, Rest API, file…）来自动化、动态的应用它的配置文件设置。

[Traefɪk]: https://traefik.io/

## 快速启动Traefik服务

```yaml
version: '3.7'

services:
  traefik:
    image: traefik
    restart: always
    labels:
      - traefik.http.routers.traefik.rule=Host(`traefik.yourdomain.com`)
      - traefik.http.routers.traefik.entrypoints=web
      - traefik.http.routers.traefik.service=api@internal
      - traefik.http.services.traefik.loadbalancer.server.port=8080
    command:
      - --providers.docker
      - --api.dashboard=true
      - --entrypoints.web.address=:80
    network_mode: host
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

<!--more-->

使用浏览器打开`http://traefik.yourdomain.com`，效果如下：

![](/images/docker-traefik.png)

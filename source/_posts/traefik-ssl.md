---
title: 在Traefik中配置Let's Encrypt
date: 2021-02-08 19:25:59
tags: [traefik, ssl, letsencrypt]
---

关于Traefik，参考我的另一篇文章：{% post_link docker-traefik 使用Docker部署Traefik服务 %}。

本文使用alidns，需要先申请`ALICLOUD_ACCESS_KEY`、`ALICLOUD_SECRET_KEY`：

```yaml
version: '3.7'

services:
  traefik:
    image: traefik
    restart: always
    labels:
      - traefik.http.routers.traefik.rule=Host(`traefik.yourdomain.com`)
      - traefik.http.routers.traefik.entrypoints=websecure
      - traefik.http.routers.traefik.service=traefik
      - traefik.http.services.traefik.loadbalancer.server.port=8080
    command:
      - --providers.docker
      - --api.dashboard=true
      - --entrypoints.web.address=:80
      - --entrypoints.web.http.redirections.entryPoint.to=websecure
      - --entrypoints.websecure.address=:443
      - --entrypoints.websecure.http.tls.certResolver=letsencrypt
      - --certificatesResolvers.letsencrypt.acme.storage=/certs/acme.json
      - --certificatesResolvers.letsencrypt.acme.dnsChallenge.provider=alidns
    environment:
      - ALICLOUD_ACCESS_KEY=xxx
      - ALICLOUD_SECRET_KEY=xxx
    network_mode: host
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - certs:/certs

volumes:
  certs:
```

更多provider参考：https://doc.traefik.io/traefik/https/acme/#providers

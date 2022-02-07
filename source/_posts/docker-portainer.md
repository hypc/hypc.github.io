---
title: 使用Docker部署Portainer系统
date: 2021-05-11 18:35:44
tags: [docker, portainer]
---

[Portainer][]是一个轻量级的docker环境管理UI，可以用来管理docker宿主机和docker swarm集群。

[Portainer]: https://www.portainer.io/

```yaml
version: '3.7'

services:
  portainer:
    image: portainer/portainer-ce
    restart: always
    labels:
      - traefik.http.routers.portainer.rule=Host(`portainer.local.hypc.host`)
      - traefik.http.routers.portainer.entrypoints=websecure
      - traefik.http.routers.portainer.service=portainer
      - traefik.http.services.portainer.loadbalancer.server.port=9000
    volumes:
      - data:/data
      - /var/run/docker.sock:/var/run/docker.sock
  portainer-agent:
    image: portainer/agent
    restart: always
    labels:
      - traefik.enable=false
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/volumes:/var/lib/docker/volumes

volumes:
  data:
```

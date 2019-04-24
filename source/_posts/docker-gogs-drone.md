---
title: Gogs + Drone搭建私有CI/CD平台
date: 2018-12-14 08:55:17
tags: [docker, gogs, drone, git]
---

```yaml
version: '2'
services:
    gogs:
        image: gogs/gogs
        container_name: gogs
        restart: always
        ports:
            - 3000:3000
        volumes:
            - ./data:/data
    drone:
        image: drone/drone:1.0.1
        container_name: drone
        restart: always
        ports:
            - 80:80
        environment:
            - DRONE_GIT_ALWAYS_AUTH=false
            - DRONE_GOGS_SERVER=http://gogs.xxxx.com
            - DRONE_RUNNER_CAPACITY=2
            - DRONE_SERVER_HOST=drone.xxxx.com
            - DRONE_SERVER_PROTO=http
            - DRONE_TLS_AUTOCERT=false
        volumes:
            - ./data:/data
            - /var/run/docker.sock:/var/run/docker.sock
```

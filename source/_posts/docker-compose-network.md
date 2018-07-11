---
title: docker-compose网络设置
date: 2018-07-11 13:01:08
tags: [docker]
---

```yml
version: '2'

services:
    main:
        image: redis:3.0
        restart: always
        networks:
            - network_name

networks:
    network_name:
        driver: bridge
        ipam:
            config:
                - subnet: 172.27.1.0/24
```

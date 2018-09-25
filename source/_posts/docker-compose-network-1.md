---
title: Docker Compose网络配置
date: 2018-09-18 14:49:30
tags: [docker]
---

配置默认网络并设置子网：

```yaml
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.17.2.0/24
```

使用已存在的网络：

```yaml
networks:
  default:
    external:
      name: other-existing-network
```

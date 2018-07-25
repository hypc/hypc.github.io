---
title: Deploying pypiserver in Docker
date: 2018-07-25 21:38:31
tags: [docker, python, pip]
---

**配置`docker-compose.yaml`文件**

```yaml
version: "2"
services:
    pypi:
        image: hypc/pypi:1.2.0-alpine
        volumes:
            - /srv/pypi:/srv/pypi
        ports:
            - 8080:80
        environment:
            - PYPI_PORT=80
            - FALLBACK_URL=https://mirrors.aliyun.com/pypi/simple/
```

**运行服务**

```bash
docker-compose up -d
```

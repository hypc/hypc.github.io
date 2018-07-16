---
title: Deploying Gogs in Docker
date: 2018-07-16 21:24:50
tags: [docker, git, gogs]
---

[Gogs][]是一个git server，下面是`docker-compose.yaml`文件：

```yaml
version: "2"
services:
  gogs:
    image: gogs/gogs
    restart: always
    ports:
      - 80:3000
    volumes:
      - ./data:/data
```


[Gogs]: https://gogs.io/

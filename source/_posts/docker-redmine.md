---
title: Deploying Redmine in Docker
date: 2019-03-05 15:15:00
tags: [docker, redmine]
---

[Redmine][]: 是一个开源的、基于Web的项目管理和缺陷跟踪工具。

下面是`docker-compose.yaml`文件：

```yaml
version: '2'
services:
    redmine:
        image: redmine:4.0
        container_name: redmine
        restart: always
        ports:
            - 3000:3000
        volumes:
            - ./redmine/sqlite:/usr/src/redmine/sqlite
            - ./redmine/plugins:/usr/src/redmine/plugins
            - ./redmine/files:/usr/src/redmine/files
            - /repos:/repos
```


[Redmine]: https://www.redmine.org/

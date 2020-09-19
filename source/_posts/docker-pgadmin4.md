---
title: 使用Docker部署PgAdmin4
date: 2020-08-09 16:09:10
tags: [docker, postgres, pgadmin]
---

在这里，我们使用[dpage/pgadmin4](https://hub.docker.com/r/dpage/pgadmin4/)镜像。

首先，配置`docker-compose.yaml`文件：

```yaml
version: '3.7'

services:
  pgadmin4:
    image: dpage/pgadmin4
    restart: always
    ports:
      - 5050:80
    volumes:
      - ./pgadmin:/var/lib/pgadmin
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@example.com
      - PGADMIN_DEFAULT_PASSWORD=123456
      - PGADMIN_CONFIG_ENHANCED_COOKIE_PROTECTION=True
      - PGADMIN_CONFIG_CONSOLE_LOG_LEVEL=10
```

在首次启动服务之前，先创建`pgadmin`目录，以便挂载到容器中：

```bash
mkdir pgadmin/
chmod 777 pgadmin
```

然后执行命令`docker-compose up -d`启动服务。

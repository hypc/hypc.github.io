---
title: 使用verdaccio搭建npmjs服务
date: 2019-05-29 08:16:30
tags: [docker, verdaccio, npm]
---

**[verdaccio][]**: A lightweight private npm proxy registry.

## Quick Start

直接使用[verdaccio/verdaccio][verdaccio/verdaccio]镜像启动服务：

```bash
docker run -it --rm --name verdaccio -p 4873:4873 verdaccio/verdaccio
```

<!--more-->

或者使用docker-compose进行部署：

```yaml
version: '3'
services:
    verdaccio:
        image: verdaccio/verdaccio
        container_name: verdaccio
        restart: always
        ports:
            - 4873:4873
```

## Usage

```bash
npm install --registry http://localhost:4783/
```

## Deploy with S3

使用S3需要用到[verdaccio-s3-storage][]插件，我们可以基于[verdaccio/verdaccio][verdaccio/verdaccio]镜像进行修改。

### Dockerfile

```dockerfile
FROM verdaccio/verdaccio

USER root
RUN npm install --production \
    && npm install verdaccio-s3-storage
COPY ./conf/config.yaml ./conf/config.yaml

USER 10001

EXPOSE 4873
CMD ["/opt/verdaccio/bin/verdaccio", "--config", "/verdaccio/conf/config.yaml", "--listen", "http://0.0.0.0:4873"]
```

### conf/config.yaml

```yaml
store:
  aws-s3-storage:
    bucket: your-bucket-name
    s3ForcePathStyle: false
    accessKeyId: *
    secretAccessKey: ***
```

### docker-compose.yaml

```yaml

version: '3'
services:
    verdaccio:
        build: .
        container_name: verdaccio
        restart: always
        ports:
            - 4873:4873
```

[verdaccio]: https://verdaccio.org/
[verdaccio/verdaccio]: https://hub.docker.com/r/verdaccio/verdaccio
[verdaccio-s3-storage]: https://github.com/Remitly/verdaccio-s3-storage

---
title: docker-compose引用环境变量
date: 2020-08-29 09:42:18
tags: [docker]
---

## Compose CLI环境变量

可以使用环境变量来配置Docker Compose CLI。例如`COMPOSE_PROJECT_NAME`、`COMPOSE_FILE`等。
更多设置请参考[][Compose CLI environment variables]。

[Compose CLI environment variables]: https://docs.docker.com/compose/reference/envvars/

## 在compose files中引用环境变量

我们可以在compose file中直接引用环境变量：

```yaml
web:
  image: "webapp:${TAG}"
```

可以通过以下两种方式设置环境变量：

1. 创建环境变量文件，然后使用`env-file`选项设置：

    ```bash
    $ cat .env.prod
    TAG=1.1.0
    $ docker-compose --env-file .env.prod up -d
    ```

2. 直接在Shell环境中设置环境变量：

    ```bash
    $ export TAG=1.1.0
    $ docker-compose up -d
    ```

<!--more-->

需要注意的是，如果没有设置环境变量，那么值将会是一个空字符串，在这种情况下，可以为该环境变量设置一个默认值：

```yaml
web:
  image: "webapp:${TAG:-1.1.0}"
```

## 将环境变量传递给容器

### 使用`environment`为容器设置环境变量

```yaml
web:
  environment:
    - DEBUG=1
```

### 使用`env_file`为容器设置环境变量

```yaml
web:
  env_file:
    - .env.prod
```

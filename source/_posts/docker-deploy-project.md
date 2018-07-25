---
title: 使用Docker部署项目
date: 2018-07-25 21:42:07
tags: [docker]
---

## 项目目录结构

```txt
proj/
    |-.docker/
        |-Dockerfile
        |-docker-compose.yaml
    |-.env
```

## Dockerfile文件编写

1. 【必须】选择一个合适的镜像，参考`FROM`命令。

2. 【可选】安装项目依赖，**项目依赖一定要在前面进行安装**。

    1. 【必须】将配置依赖的配置文件拷贝到响应目录下，参考`COPY`命令。

    2. 【必须】设置工作目录，参考`WORKDIR`命令。

    3. 【必须】安装项目依赖，参考`RUN`命令。

3. 【可选】设置docker build参数，参考`ARG`命令。

4. 【必须】将项目代码拷贝到响应目录，参考`ADD`命令。

5. 【必须】设置工作目录，如果2没有执行的话，参考`WORKDIR`命令。

6. 【可选】编译项目，如果必须的话，参考`RUN`命令。

7. 【可选】设置挂载目录，如果必须的话，参考`VOLUME`命令。

8. 【必须】设置需要暴露的端口号，参考`EXPOSE`命令。

9. 【必须】设置服务启动时运行的命令，参考`CMD`命令。

<!--more-->

**示例1：一个Python项目的示例**

```dockerfile
FROM python:3.5-alpine

COPY requirements.txt /app/requirements.txt
WORKDIR /app
RUN pip install -r requirements.txt

ADD . /app

EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

**示例2：一个Angular2项目的示例**

```dockerfile
FROM node:6-alpine

COPY package.json /app/package.json
WORKDIR /app
RUN npm install -g @angular/cli http-server \
    && npm install

ADD . /app
RUN ng build

EXPOSE 80
CMD ["http-server", "/app/dist", "-p", "80"]
```

## docker-compose.yaml文件编写

1. 【必须】设置compose文件的版本，参数`version`。

2. 【必须】设置服务列表，下面可以有多个服务，参数`services`。

3. 【必须】设置服务名，服务名即是参数名。

4. 【必须】设置build相关参数，参数`build`。

    1. 【必须】设置Dockerfile文件的位置，参数`context`。

    2. 【可选】设置Dockerfile的文件名，如果文件就是`Dockerfile`可不设置，参数`dockerfile`。

    3. 【可选】设置编译时需要的参数列表，至少设置一个参数，否则不设置，参数`args`。

        冒号分隔，冒号前是参数名，后面是参数值。

5. 【必须】设置镜像名称，参数`image`。

6. 【可选】设置依赖的服务列表，服务名参考第3步设置。参数`depends_on`。

7. 【必须】设置绑定的端口号，参数`ports`。

    冒号分隔，冒号前面是宿主机的端口，冒号后面是docker容器的端口。
    容器的端口后面也可以指定协议，如：`8000:8000`、`53:53/tcp`、`53:53/udp`。

8. 【必须】设置服务意外停止时重启设置，参数`restart`。

9. 【可选】如果不实用镜像自带命令，可在这设置启动服务运行的命令，参数`command`。

10. 【可选】设置服务运行时需要环境变量配置文件，参数`env_file`。

11. 【可选】设置挂载列表，可以将文件或文件夹挂载到容器中，参数`volumes`。

    冒号分隔，前面是宿主机文件/文件夹，后面是容器里面的文件/文件夹。

**示例**

```yaml
version: "2"
services:
    cloud:
        build:
            context: ..
            dockerfile: Dockerfile
            args:
                debug: true
        image: cloud
        depends_on:
            - memcached
        ports:
            - 8000:8000
        restart: always
        env_file: ./environment-d
    memcached:
        image: memcached:latest
        restart: always
```

## .env文件编写

1. 【必须】设置COMPOSE_PROJECT_NAME。

**示例**

```
COMPOSE_PROJECT_NAME=cloud
```

## 项目部署

```shell
# 部署时执行
docker-compose -f .docker/docker-compose.yaml up -d --build
# 重启时执行
docker-compose -f .docker/docker-compose.yaml restart
```

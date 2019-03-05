---
title: Deploying Jenkins in Docker
date: 2019-03-01 08:27:35
tags: [docker, jenkins]
---

[Jenkins][]: 是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

下面是`docker-compose.yaml`文件：

```yaml
version: "2"
services:
    jenkins:
        image: jenkinsci/jenkins:lts
        container_name: jenkins
        restart: always
        ports:
            - 8080:8080
            - 50000:50000
        volumes:
            - ./jenkins:/var/jenkins_home
```


[Jenkins]: https://jenkins.io/

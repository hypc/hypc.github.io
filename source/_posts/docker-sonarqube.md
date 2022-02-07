---
title: 使用Docker部署SonarQube
date: 2021-02-22 16:08:08
tags: [docker, sonarqube, jenkins]
---

[SonarQube][]是一个开源的代码质量管理平台，专用于持续集成分析和测量技术质量，从项目的组合到方法。

[SonarQube]: https://www.sonarqube.org/

## 部署SonarQube

```yaml
version: '3.7'

services:
  sonarqube:
    image: sonarqube
    restart: always
    labels:
      - traefik.http.routers.sonarqube.rule=Host(`sonarqube.yourdomain.com`)
      - traefik.http.routers.sonarqube.entrypoints=websecure
      - traefik.http.routers.sonarqube.service=sonarqube
      - traefik.http.services.sonarqube.loadbalancer.server.port=9000
    environment:
      - SONAR_JDBC_URL=jdbc:postgresql://postgres/sonarqube
      - SONAR_JDBC_USERNAME=postgres
      - SONAR_JDBC_PASSWORD=postgres
    volumes:
      - extensions:/opt/sonarqube/extensions

volumes:
  extensions:
```

<!--more-->

## 在Jenkins中集成SonarQube

### 安装 SonarQube Scanner 插件

### 配置Jenkins

1. `系统管理 -> 系统配置 -> SonarQube servers`
    - Name: SonarQube
    - Server URL: https://sonarqube.yourdomain.com
    - Server authentication token: 在SonarQube上申请的token
2. `系统管理 -> 全局工具配置 -> SonarQube Scanner`
    - Name: Scanner
    - 自动安装: √
    - Install from Maven Central: 选择最新版

### 添加任务

`构建 -> Execute SonarQube Scanner -> Analysis properties`

```ini
sonar.projectKey=$project_name
sonar.sources=.
sonar.exclusions=build/*,dist/*,docs/*
```

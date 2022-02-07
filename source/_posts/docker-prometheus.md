---
title: 使用Docker部署Prometheus+Grafana服务
date: 2020-10-10 19:14:12
tags: [docker, prometheus, grafana]
---

## Prometheus简介

[Prometheus][]是一套开源的系统监控报警框架。它启发于Google的borgmon监控系统，
由工作在SoundCloud的google前员工在2012年创建，作为社区开源项目进行开发，并于2015年正式发布。
2016年，Prometheus正式加入Cloud Native Computing Foundation，成为受欢迎度仅次于Kubernetes的项目。

[Prometheus]: https://prometheus.io/

<img src="/images/docker-prometheus.png" style="max-width: 80%;">

<!--more-->

## Grafana简介

[Grafana][]是一个跨平台的开源的度量分析和可视化工具，可以通过将采集的数据查询然后可视化的展示，并及时通知。

[Grafana]: https://grafana.com/

<video controls autoplay src="/images/docker-grafana.mp4" style="max-width: 80%;"></video>

## 安装

创建Prometheus配置文件`prometheus.yaml`，内容如下：

```yaml
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s
alerting:
  alertmanagers:
  - scheme: http
    timeout: 10s
    api_version: v1
    static_configs:
    - targets: []
scrape_configs:
- job_name: prometheus
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - localhost:9090
- job_name: node
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  static_configs:
  - targets:
    - node_exporter:9100
```

创建`docker-compose.yaml`文件，内容如下：

```yaml
version: '3.7'
services:
  grafana:
    image: grafana/grafana
    restart: always
    ports:
      - 3000:3000
  prometheus:
    image: prom/prometheus
    restart: always
    ports:
      - 9090:9090
    command: --config.file=/etc/prometheus/prometheus.yaml
    volumes:
      - ./prometheus.yaml:/etc/prometheus/prometheus.yaml
  node_exporter:
    image: prom/node-exporter
    restart: always
    command: --path.rootfs=/host
    volumes:
      - /:/host:ro,rslave
```

使用`docker-compose up -d`命令启动服务。

## 配置Grafana

浏览器打开 http://localhost:3000/ ，使用默认账号`admin/admin`登录。
然后添加Prometheus数据源，并导入默认的Dashboards。

<video controls autoplay src="/images/docker-grafana-prometheus.mp4" style="max-width: 80%;"></video>

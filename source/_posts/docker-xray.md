---
title: 使用Docker部署XRay服务
date: 2021-02-27 14:55:23
tags: [docker, proxy]
---

[XRay][]是一款原生支持XTLS黑科技且源自[V2Ray][]却超越V2Ray的科学上网工具。

[XRay]: https://xtls.github.io/
[V2Ray]: https://www.v2ray.com/

## 服务部署

`docker-compose.yml`文件：

```yaml
version: '3.7'

services:
  xray:
    image: teddysun/xray
    restart: always
    labels:
      - traefik.http.routers.xray.rule=Host(`xray.yourdomain.com`) && Path(`/vxknSD77hYanwwRM`)
      - traefik.http.routers.xray.entrypoints=websecure
      - traefik.http.routers.xray.service=xray
      - traefik.http.services.xray.loadbalancer.server.port=9000
    volume:
      - ./config.json:/etc/xray/config.json
```

<!--more-->

`config.json`文件：

```json
{
  "inbound": {
    "port": 9000,
    "protocol": "vmess",
    "settings": {
      "clients": [{ "id": "01605FB4-129A-4673-BDD8-D7BC592B404A" }]
    },
    "streamSettings": {
      "network": "ws",
      "wsSettings": { "path": "/vxknSD77hYanwwRM" }
    }
  },
  "outbound": { "protocol": "freedom" }
}
```

## 客户端配置

下载并安装客户端，按以下内容进行配置：

* id: 01605FB4-129A-4673-BDD8-D7BC592B404A
* wss://xray.yourdomain.com/vxknSD77hYanwwRM

---
title: 使用FRP搭建内网穿透服务
date: 2019-05-29 13:46:38
tags: [docker, frp]
---

[frp][]是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力，且尝试性支持了点对点穿透。

本文主要内容是『如何利用内网穿透访问内网Web服务』。

## 准备工作

* 一台有公网ip的服务器S1，ip为：x.x.x.x
* 一台位于内网（没有公网ip）的服务器S2
* 一个已经解析到公网ip的域名：test.yourdomain.com
* 一个位于内网的Web服务，地址为：y.y.y.y:local_port

<!--more-->

## 部署frps服务

frps服务部署在S1服务器上，执行下面命令：

```bash
cat <<"EOF">> frps.ini
[common]
bind_addr = 0.0.0.0
bind_port = 7000
vhost_http_port = 8000
token = 1234567890123456
dashboard_addr = 0.0.0.0
dashboard_port = 7500
dashboard_user = admin
dashboard_pwd = admin
EOF

cat <<"EOF">> docker-compose.yaml
version: '2'
services:
    frps:
        image: riftbit/frp-server
        container_name: frps
        restart: always
        ports:
            - 7000:7000
            - 8000:8000
            - 7500:7500
        volumes:
            - ./frps.ini:/frp_config/frps.ini
EOF

docker-compose up -d
```

## 部署frpc服务

frpc服务部署在S2服务器上，执行下面命令：

```bash
cat <<"EOF">> frpc.ini
[common]
server_addr = x.x.x.x
server_port = 7000
token = 1234567890123456

[web]
type = http
local_ip = y.y.y.y
local_port = local_port
custom_domains = test.yourdomain.com
EOF

cat <<"EOF">> docker-compose.yaml
version: '2'
services:
    frpc:
        image: riftbit/frp-client
        container_name: frpc
        restart: always
        volumes:
            - ./frpc.ini:/frp_config/frpc.ini
EOF

docker-compose up -d
```

## 访问

使用浏览器打开`http://test.yourdomain.com:8000`访问位于内网的Web服务。

## 后记

可以在S1服务器上使用nginx做一下反向代理，然后可以直接使用`http://test.yourdomain.com`访问Web服务。


[frp]: https://github.com/fatedier/frp

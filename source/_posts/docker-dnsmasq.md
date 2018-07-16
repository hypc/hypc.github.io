---
title: 使用Docker搭建dnsmasq服务
date: 2018-07-16 21:21:57
tags: [docker, dnsmasq]
---

```bash
# 创建配置文件目录
mkdir configs

cat <<EOF>> configs/addresses.conf
# Add domains which you want to force to an IP address here.
# The example below send any host in double-click.net to a local
# web-server.
#address=/double-click.net/127.0.0.1

# --address (and --server) work with IPv6 addresses too.
#address=/www.thekelleys.org.uk/fe80::20d:60ff:fe36:f83
EOF

# 创建docker-compose.yml文件
cat <<EOF>> docker-compose.yml
version: "2"
services:
    dnsmasq:
        image: hypc/dnsmasq:2.76-alpine
        ports:
            - 53:53/tcp
            - 53:53/udp
        restart: always
        volumes:
            - ./configs:/etc/dnsmasq.d
        cap_add:
            - NET_ADMIN
EOF
```

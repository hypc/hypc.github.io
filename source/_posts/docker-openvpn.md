---
title: Installing OpenVPN in Docker
date: 2018-07-16 21:16:59
tags: [docker, openvpn, vpn]
---

## 准备工作

本教程主要讲怎么使用`docker`及`docker-compose`搭建一个OpenVPN服务器，需要进行以下操作：

1. 安装`docker`服务，参考[Install Docker][]；
2. 安装`docker-compose`工具，参考[Install Docker Compose][]；
3. 拉取镜像`kylemanna/openvpn`。

## 安装并运行OpenVPN服务

### 设置环境变量

```bash
# 设置OpenVPN数据存放位置
OVPN_DATA=`pwd`/ovpn_data
# 设置OpenVPN域名，也可以设置为ip
OVPN_DOMAIN=openvpn.example.com
```

### 初始化`OVPN_DATA`

```bash
docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://$OVPN_DOMAIN
docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki
```

### 编写`docker-compose.yaml`文件

```yaml
version: '2'
services:
    openvpn:
        image: kylemanna/openvpn
        ports:
            - 1194:1194/udp
        restart: always
        volumes:
            - ./ovpn_data:/etc/openvpn
        cap_add:
            - NET_ADMIN
```

### 启动OpenVPN服务

```bash
docker-compose up -d
```

## 创建客户端

假设客户端名称为：`CLIENTNAME`。

### 创建客户端

```bash
docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
```

### 导出客户端配置文件

```bash
docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn
```


[OpenVPN]: https://openvpn.net/
[Install Docker]: https://docs.docker.com/engine/installation/
[Install Docker Compose]: https://docs.docker.com/compose/install/

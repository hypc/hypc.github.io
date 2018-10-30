---
title: 使用Docker部署zabbix服务
date: 2018-09-26 18:52:02
tags: [docker, zabbix]
---

[zabbix][]是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。

[zabbix][]能监视各种网络参数，保证服务器系统的安全运营；并提供灵活的通知机制以让系统管理员快速定位以及解决存在的各种问题。

zabbix由2部分构成，zabbix server与可选组件zabbix agent。

zabbix server可以通过SNMP，zabbix agent，ping，端口监视等方法提供对远程服务器、网络状态的监视，数据收集等功能，
它可以运行在Linux，Solaris，HP-UX，AIX，Free BSD，Open BSD，OS X等平台上。

<!--more-->

## zabbix server部署

```yaml
version: '3'
services:
    zabbix:
        image: zabbix/zabbix-web-nginx-mysql:alpine-3.4-latest
        container_name: zabbix
        restart: always
        ports:
            - 8084:80
        environment:
            - ZBX_SERVER_HOST=zabbix_ser
            - DB_SERVER_HOST=zabbix_db
            - MYSQL_USER=zabbix
            - MYSQL_PASSWORD=zabbix
            - MYSQL_DATABASE=zabbix
            - PHP_TZ=Asia/Shanghai
        depends_on:
            - zabbix_db
            - zabbix_ser
    zabbix_ser:
        image: zabbix/zabbix-server-mysql:alpine-3.4-latest
        container_name: zabbix_ser
        restart: always
        ports:
            - 10051:10051
        environment:
            - DB_SERVER_HOST=zabbix_db
            - MYSQL_USER=zabbix
            - MYSQL_PASSWORD=zabbix
            - MYSQL_DATABASE=zabbix
        depends_on:
            - zabbix_db
    zabbix_db:
        image: mysql:5.7
        container_name: zabbix_db
        restart: always
        environment:
            - MYSQL_ROOT_PASSWORD=zabbix
            - MYSQL_DATABASE=zabbix
            - MYSQL_USER=zabbix
            - MYSQL_PASSWORD=zabbix
        volumes:
            - ./data:/var/lib/mysql
networks:
    default:
        driver: bridge
        ipam:
            driver: default
            config:
                - subnet: 172.27.4.0/24
```

**注意**：启动服务时需要依次启动：`zabbix_db`、`zabbix_ser`、`zabbix`。

## zabbix agent部署

```yaml
version: '3'
services:
    zabbix_agent:
        image: zabbix/zabbix-agent:alpine-3.4-latest
        container_name: zabbix_agent
        restart: always
        ports:
            - 10050:10050
        environment:
            - ZBX_HOSTNAME=10.1.50.112
            - ZBX_SERVER_HOST=zabbix_ser
            - ZBX_PASSIVESERVERS=172.27.5.1
networks:
    default:
        driver: bridge
        ipam:
            driver: default
            config:
                - subnet: 172.27.5.0/24
```

[zabbix]: https://www.zabbix.com/

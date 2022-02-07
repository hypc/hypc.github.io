---
title: AdGuard Home
date: 2022-01-03 20:02:06
tags: [adguard]
---

## Install

1. 直接使用下面 `docker-compose.yaml` 启动服务：

    ```yaml
    version: '3.7'

    services:
      adguard:
        image: adguard/adguardhome
        container_name: adguard
        restart: always
        labels:
          - traefik.http.routers.adguard.rule=Host(`adguard.domain.com`)
          - traefik.http.routers.adguard.entrypoints=websecure
          - traefik.http.routers.adguard.service=adguard
          - traefik.http.services.adguard.loadbalancer.server.port=3000
        network_mode: host
        volumes:
          - ./config:/opt/adguardhome/conf
    ```

2. 使用浏览器打开页面 https://adguard.domain.com ，根据向导初始化 AdGuard 服务

3. 修改 `config/AdGuardHome.yaml` 文件，将 `bind_port` 修改为 `3000`，然后重启服务

4. 再次打开网页 https://adguard.domain.com ，进入 Dashboard 页面

<!--more-->

## 进一步设置

```markmap
* AdGuard Home 设置
    * 常规设置
        * 过滤器更新时间
        * 浏览安全
        * 日志配置
            * 日志保留时间
            * 统计保留时长
    * DNS 设置
        * 上游 DNS 服务器
        * Bootstrap DNS 服务器
        * DNS 查询速度限制
    * DNS 封锁清单
    * DNS 重写
```

**上游 DNS 服务**

```txt
tls://dns.google
tls://1.1.1.1
https://1.1.1.1/dns-query
https://dns.cloudflare.com/dns-query
tls://dns.alidns.com
https://dns.alidns.com/dns-query
tls://doh.pub
https://doh.pub/dns-query
https://dns.pub/dns-query
```

**Bootstrap DNS 服务器**

```txt
9.9.9.9
8.8.8.8
1.1.1.1
223.5.5.5
119.29.29.29
```

**DNS 封锁清单**

启用默认的拦截清单，并添加以下阻止列表：

* https://anti-ad.net/adguard.txt
* https://anti-ad.net/easylist.txt

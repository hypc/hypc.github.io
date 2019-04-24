---
title: 使用Privoxy将Socks代理转为Http代理
date: 2019-04-19 13:13:55
tags: [proxy, privoxy]
---

[Privoxy][]是一款带过滤功能的代理服务器，针对HTTP、HTTPS协议。
通过Privoxy的过滤功能，用户可以保护隐私、对网页内容进行过滤、管理cookies，以及拦阻各种广告等。

[Privoxy]: http://www.privoxy.org/

## 安装Privoxy

ubuntu:

```bash
sudo apt-get install -y privoxy
sudo systemctl enable privoxy
sudo systemctl start privoxy
```

centos:

```bash
sudo yum install -y privoxy
sudo systemctl enable privoxy
sudo systemctl start privoxy
```

<!--more-->

## 转发socks代理

打开Privoxy的配置文件`/etc/privoxy/config`，找到下面这一行：

```
# forward-socks5 / 127.0.0.1:9050 .
```

去掉前面的`#`号，并将后面的ip和port改成socks服务的ip和port，然后重启服务：

```bash
sudo systemctl start privoxy
```

## 允许其他设备使用

打开Privoxy的配置文件`/etc/privoxy/config`，找到下面这一行：

```
listen-address  127.0.0.1:8118
```

将其中的ip修改为`0.0.0.0`，然后重启服务：

```bash
sudo systemctl start privoxy
```

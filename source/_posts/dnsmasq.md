---
title: OSX系统安装DNSmasq服务
date: 2018-07-05 16:26:55
tags: [dnsmasq, osx]
---

DNSmasq是一个小巧且方便地用于配置DNS和DHCP的工具，适用于小型网络，它提供了DNS功能和可选择的DHCP功能。
它服务那些只在本地适用的域名，这些域名是不会在全球的DNS服务器中出现的。
DHCP服务器和DNS服务器结合，并且允许DHCP分配的地址能在DNS中正常解析，
而这些DHCP分配的地址和相关命令可以配置到每台主机中，也可以配置到一台核心设备中（比如路由器），
DNSmasq支持静态和动态两种DHCP配置方式。

## 安装DNSmasq

### 安装

```bash
brew install dnsmasq
```

### 配置

**拷贝默认配置文件**

```bash
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
```

**设置dnsmasq的DNS服务**

打开文件`/usr/local/etc/resolv.dnsmasq.conf`，设置以下内容：

```
# google dns
nameserver 8.8.8.8
# open dns
nameserver 208.67.222.222
nameserver 208.67.220.220
```

打开文件`/usr/local/etc/dnsmasq.conf`，设置以下内容：

```
resolv-file=/usr/local/etc/resolv.dnsmasq.conf
address=/xxx.com/127.0.0.1
```

### 启动服务

```bash
sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
```

### 测试

```bash
dig @127.0.0.1 xxx.com
```

使用时需要将本机DNS改为`127.0.0.1`。

## 重启DNSmasq

如果修改了配置文件，可以通过下面命令重启dnsmasq：

```bash
sudo launchctl stop homebrew.mxcl.dnsmasq
sudo launchctl start homebrew.mxcl.dnsmasq
```

使用线面命令清除dns缓存：

```bash
sudo killall -HUP mDNSResponder
```

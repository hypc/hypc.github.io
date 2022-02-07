---
title: VMware Fusion中虚拟机的网络设置
date: 2020-11-07 18:16:19
tags: [vmware]
---

## VMware网络模式

VMware虚拟机有三种网络模式：

* Bridged - 桥接模式
* NAT - 网络地址转换模式
* Host-Only - 仅主机模式

### Bridged - 桥接模式

通过桥接模式网络连接，虚拟机中的虚拟网络适配器可连接到主机系统中的物理网络适配器。
虚拟机可通过主机网络适配器连接到主机系统所用的LAN。

桥接模式网络连接将虚拟机配置为在网络中具有唯一标识，与主机系统相分离，且与主机系统无关。
虚拟机可完全参与到网络活动中。它能够访问网络中的其他计算机，也可以被网络中的其他计算机访问，就像是网络中的物理机那样。

### NAT - 网络地址转换模式

使用NAT模式网络时，虚拟机在外部网络中不必具有自己的IP地址。
主机系统上会建立单独的专用网络。在默认配置中，虚拟机会在此专用网络中通过DHCP服务器获取地址。

<!--more-->

虚拟机和主机系统共享一个网络标识，此标识在外部网络中不可见。
NAT工作时会将虚拟机在专用网络中的IP地址转换为主机系统的IP地址。
当虚拟机发送对网络资源的访问请求时，它会充当网络资源，就像请求来自主机系统一样。

主机系统在NAT网络上具有虚拟网络适配器。借助该适配器，主机系统可以与虚拟机相互通信。
NAT设备可在一个或多个虚拟机与外部网络之间传送网络数据，识别用于每个虚拟机的传入数据包，并将它们发送到正确的目的地。

### Host-Only - 仅主机模式

虚拟机和主机系统之间的网络连接由对主机操作系统可见的虚拟网络适配器提供。虚拟DHCP服务器可在仅主机模式网络中提供IP地址。

在默认配置中，仅主机模式网络中的虚拟机无法连接到Internet。

## VMware Fusion中虚拟机的网络设置

VMware Fusion网络配置文件位于`/Library/Preferences/VMware Fusion/`目录下：

```
/Library/Preferences/VMware Fusion/networking
/Library/Preferences/VMware Fusion/vmnet8/dhcpd.conf
/Library/Preferences/VMware Fusion/vmnet8/nat.conf
/Library/Preferences/VMware Fusion/vmnet1/dhcpd.conf
```

### 先停止vmware网络服务

```bash
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
```

### 修改`networking`配置文件

打开`networking`文件，内容如下，可自行修改其中内容或者添加新的网络配置：

```
VERSION=1,0
answer VNET_1_DHCP yes
answer VNET_1_DHCP_CFG_HASH 18A2350F98C82E904AB23D4E76216FD47B9D99FB
answer VNET_1_HOSTONLY_NETMASK 255.255.255.0
answer VNET_1_HOSTONLY_SUBNET 192.168.21.0
answer VNET_1_VIRTUAL_ADAPTER yes
answer VNET_8_DHCP yes
answer VNET_8_DHCP_CFG_HASH A78BA28C18BC23DE29320723366DDB102AD7DF26
answer VNET_8_HOSTONLY_NETMASK 255.255.255.0
answer VNET_8_HOSTONLY_SUBNET 192.168.127.0
answer VNET_8_NAT yes
answer VNET_8_VIRTUAL_ADAPTER yes
```

### 配置网络

执行下面命令：

```bash
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --configure
```

`vmnet-cli`会根据`networking`文件的内容自动修改`dhcpd.conf`、`nat.conf`文件内容。

### 启动网络服务

```bash
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
```

### 为虚拟机配置静态ip

打开`vmnet8/dhcpd.conf`文件，在最后加上一下内容：

```
host centos-docker-server {
        hardware ethernet 00:50:56:c0:00:01;
        fixed-address 192.168.127.10;
        option broadcast-address 192.168.127.255;
        option domain-name-servers 192.168.127.2;
        option domain-name local;
        option routers 192.168.127.2;
}
```

然后重启网络服务：

```bash
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --stop
sudo /Applications/VMware\ Fusion.app/Contents/Library/vmnet-cli --start
```

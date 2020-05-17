---
title: CentOS8初始化设置
date: 2020-02-12 13:01:03
tags: [centos]
---

从官网下载最新镜像并执行离线最小化安装。

## 安装常用软件

```bash
## 激活网卡
sed -i 's/^ONBOOT=.*$/ONBOOT=yes/' /etc/sysconfig/network-scripts/*
## 配置yum源
mv /etc/yum.repos.d /etc/yum.repos.d.bak
mkdir /etc/yum.repos.d
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-8.repo
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo
yum makecache

## 安装常用软件
yum update -y && yum install -y vim git tree unzip rsync samba bind-utils
## 安装vim插件
curl -o- https://raw.githubusercontent.com/hypc/vimfiles/master/install.simple.sh | bash

## docker
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y --nobest docker-ce containerd.io
systemctl enable docker && systemctl start docker
## docker-compose
curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

<!--more-->

## 允许root用户远程登陆

修改`/etc/ssh/sshd_config`文件，设置以下参数：

```
PermitRootLogin yes
```

然后开启并运行`sshd`服务：

```bash
systemctl enable sshd && systemctl start sshd
```

## 网络管理命令

* `ip`: 替代`ifconfig`命令
* `ss`: 替代`netstat`命令

## 磁盘管理

使用`fdisk`命令进行分区，使用`mke2fs`命令进行格式化。

```bash
echo '/dev/sdb1 /mnt/nas ext4 defaults 0 2' >> /etc/fstab   # 开机自动挂载磁盘
```

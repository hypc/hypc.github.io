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
yum makecache

## 安装常用软件
yum update -y && yum install -y vim git tree unzip
## 安装vim插件
curl -o- https://raw.githubusercontent.com/hypc/vimfiles/master/install.simple.sh | bash

## docker
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce
mkdir -p /etc/docker && tee /etc/docker/daemon.json <<"EOF"
{
    "registry-mirrors": ["https://icxw8635.mirror.aliyuncs.com"]
}
EOF
systemctl enable --now docker
## docker-compose
curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

<!--more-->

## 允许root用户远程登陆

修改`/etc/ssh/sshd_config`文件，设置以下参数：

```
PermitRootLogin yes
```

然后启动`sshd`服务：

```bash
systemctl enable --now sshd
```

## 网络管理命令

* `ip`: 替代`ifconfig`命令
* `ss`: 替代`netstat`命令

## 磁盘管理

使用`fdisk`命令进行分区，使用`mke2fs`命令进行格式化。

```bash
echo '/dev/sdb1 /mnt/a ext4 defaults 0 0' >> /etc/fstab   # 开机自动挂载磁盘
```

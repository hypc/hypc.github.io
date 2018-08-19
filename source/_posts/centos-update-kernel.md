---
title: Centos7安装并使用新内核
date: 2018-08-19 14:05:04
tags: [centos]
---

## 安装最新稳定内核

首先通过命令`uname -r`查看当前系统的内核版本号，我的是：

```txt
3.10.0-123.4.2.el7.x86_64
```

安装`ELRepo`仓库：

```bash
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

安装最新稳定的内核版本：

```bash
sudo yum --enablerepo=elrepo-kernel install -y kernel-ml
```

使用命令`rpm -qa | grep kernel`查看是否安装成功，我的系统显示如下：

```txt
kernel-ml-4.16.2-1.el7.elrepo.x86_64
kernel-tools-libs-3.10.0-514.26.2.el7.x86_64
kernel-tools-3.10.0-514.26.2.el7.x86_64
kernel-3.10.0-123.4.2.el7.x86_64
kernel-3.10.0-123.el7.x86_64
kernel-3.10.0-514.26.2.el7.x86_64
```

可以看到里面的kernel-ml-4.*，证明安装成功。

<!--more-->

## 设置启动顺序

执行命令：

```bash
sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
```

运行结果如下：

```txt
CentOS Linux (4.16.2-1.el7.elrepo.x86_64) 7 (Core)
CentOS Linux 7 Rescue 6f1c5ebde1e74d999138a767abf2ef2b (3.10.0-514.26.2.el7.x86_64)
CentOS Linux (3.10.0-514.26.2.el7.x86_64) 7 (Core)
CentOS Linux (3.10.0-123.4.2.el7.x86_64) 7 (Core)
CentOS Linux, with Linux 3.10.0-123.el7.x86_64
CentOS Linux, with Linux 0-rescue-11264912be38456483e63dfd21d402f4
```

可以看到排在最上面的就是最新的`4.*`的内核，从上往下序号依次是：0、1、2、3、4、5，
看最新版本是在什么位置，然后通过下面命令设置启动顺序：

```bash
sudo grub2-set-default 0
```

最后重新系统，通过命令`uname -r`查看当前系统的内核版：

```txt
4.16.2-1.el7.elrepo.x86_64
```

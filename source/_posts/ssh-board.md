---
title: SSH穿越跳板机登录远程服务器
date: 2019-11-01 15:47:49
tags: [ssh, shell]
---

如果公司服务器处于外网，而出于安全方面的考虑，访问公司外网服务器时一般会通过跳板机（堡垒机）进行访问。

![](/images/ssh-board-1.svg)

## 方式1

使用`ssh`分别登录：

```bash
# 先登录到跳板机
$ ssh -A username@192.168.0.3   # -A: 表示转发密钥，它会将本机的密钥转发到跳板机上，从跳板机登录目标机器时会使用该密钥
# 从跳板机上再登录目标机器
$ ssh username@10.0.1.10
```

## 方式2

使用`ProxyCommand`选项，一行命令直接登录到目标机器：

```bash
$ ssh username@10.0.1.10 -o ProxyCommand='ssh username@192.168.0.3 -W %h:%p'
```

<!--more-->

命令有点复杂，推荐使用`~/.ssh/config`配置文件：

```
Host    proxy.machine
    HostName        192.168.0.3
    Port            22
    User            username
    IdentityFile    ~/.ssh/id_rsa
    ProxyCommand    none
    CheckHostIP     no
    ForwardAgent    yes
    ControlMaster   auto
    ControlPersist  10m

Host    10.0.1.*
    Port            22
    User            username
    IdentityFile    ~/.ssh/id_rsa
    ProxyCommand    ssh username@proxy.machine -W %h:%p
```

然后使用`ssh 10.0.1.10`登录目标机器。

## 多重跳板机

![](/images/ssh-board-2.svg)

```
Host    proxy1.machine
    HostName        192.168.0.3
    Port            22
    User            username
    IdentityFile    ~/.ssh/id_rsa
    ProxyCommand    none
    CheckHostIP     no
    ForwardAgent    yes
    ControlMaster   auto
    ControlPersist  10m

Host    proxy2.machine
    HostName        1.2.3.4
    Port            22
    User            username
    IdentityFile    ~/.ssh/id_rsa
    ProxyCommand    ssh username@proxy1.machine -W %h:%p
    CheckHostIP     no
    ForwardAgent    yes
    ControlMaster   auto
    ControlPersist  10m

Host    10.0.1.*
    Port            22
    User            username
    IdentityFile    ~/.ssh/id_rsa
    ProxyCommand    ssh username@proxy2.machine -W %h:%p
```

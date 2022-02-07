---
title: k3s集群中开启firewall
date: 2021-07-16 20:27:40
tags: [k3s, kubernets]
---

关于如何搭建k3s集群，可以参考我的{% post_link k3s-cluster "上一篇" %}文章，本文介绍如何在k3s集群中开启firewall。

下面是一些常用的端口：

PROTOCOL    | PORT          | DESCRIPTION
------------|---------------|------------------------------
TCP         | 2376          | Node driver Docker daemon TLS port
TCP         | 2379          | Etcd client requests
TCP         | 2380          | Etcd peer communication
TCP         | 6443          | Kubernetes API
UDP         | 8472          | Canal/Flannel VXLAN overlay networking
TCP         | 9099          | Canal/Flannel livenessProbe/readinessProbe
TCP         | 10250         | Kubelet API
TCP         | 10254         | Ingress controller livenessProbe/readinessProbe
TCP / UDP   | 30000-32767   | NodePort port range

<!--more-->

**k3s Master**

```bash
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=8472/udp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=30000-32767/tcp
firewall-cmd --permanent --add-port=30000-32767/udp
firewall-cmd --permanent --add-masquerade
firewall-cmd --reload
```

**k3s Agent**

```bash
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd --permanent --add-port=8472/udp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=30000-32767/tcp
firewall-cmd --permanent --add-port=30000-32767/udp
firewall-cmd --permanent --add-masquerade
firewall-cmd --reload
```

---
title: MicroK8s问题集
date: 2018-12-28 16:39:35
tags: [kubernetes, docker]
---

> ContainerCreating

当执行`kubectl get pod -l app=xxx`时，容器一直处于ContainerCreating状态，
使用`kubectl describe pod xxx-xxx`查看pod信息。

----

> kubelet does not have ClusterDNS IP configured and cannot create Pod using "ClusterFirst" policy. Falling back to "Default" policy.

没有启动dns服务，使用命令`microk8s.enable dns dashboard`启动dns。

----

> code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)

这是由于无法连接到国外服务器造成的，可以先从国内的服务器上拉取相应的镜像，然后修改成相应的版本即可。<!--more-->

```bash
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/pause-amd64:3.1
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/kube-proxy-amd64:v1.11.3
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/kube-scheduler-amd64:v1.11.3
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/coredns:1.1.3
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/pause:3.1
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/kube-controller-manager-amd64:v1.11.3
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/kube-apiserver-amd64:v1.11.3
docker pull registry.cn-beijing.aliyuncs.com/zhoujun/etcd-amd64:3.2.18
```

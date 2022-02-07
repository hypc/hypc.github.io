---
title: k3s容器之间互相连接
date: 2021-08-17 17:36:35
tags: [k3s, kubernetes]
---

## pod内容器之间相互访问

由于 pod 内容器共享一个网络命名空间（network namespace），
故 pod 内容器之间可以直接使用 `localhost` 互相访问。

## pod之间容器相互访问

| address                                        | description                     |
| ---------------------------------------------- | ------------------------------- |
| `{service_name}.{namespace}.svc.cluster.local` | `cluster.local` 指的是集群域名  |
| `{service_name}.{namespace}.svc`               | 集群内访问 svc                  |
| `{service_name}.{namespace}`                   | 集群内访问 svc                  |
| `{service_name}`                               | 集群内同一 namespace 内访问 svc |

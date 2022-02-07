---
title: 允许 k3s 访问外网
date: 2021-08-05 13:10:26
tags: [k3s, kubernetes]
---

默认情况下，Pod 采用的是 `dnsPolicy: ClusterFirst` 策略，
故 k3s 中 [coredns][] 不会解析外部服务域名，修改 [coredns][] 的 `ConfigMap` 配置：

[coredns]: https://coredns.io/

![](/images/k3s-extranet.png)

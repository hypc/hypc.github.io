---
title: k3s执行helm报错：Kubernetes cluster unreachable
date: 2021-07-07 10:16:26
tags: [k3s, kubernetes]
---

> Error: Kubernetes cluster unreachable: Get "http://localhost:8080/version?timeout=32s": dial tcp [::1]:8080: connect: connection refused

**报错原因**：

helm v3版本不再需要Tiller，而是直接访问ApiServer来与k8s交互，通过环境变量`KUBECONFIG`来读取存有ApiServre的地址与token的配置文件地址，默认地址为`~/.kube/config`。

**解决办法**：

* 通过修改环境变量`KUBECONFIG`来解决这个问题：`export KUBECONFIG=/etc/rancher/k3s/k3s.yaml`。
* 或者`ln -s /etc/rancher/k3s/k3s.yaml ~/.kube/config`。

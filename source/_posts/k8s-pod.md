---
title: Kubernetes名词解释之Pod
date: 2019-07-12 09:02:25
tags: [kubernetes]
---

Pod是Kubernetes应用程序的基本执行单元 - Kubernetes应用程序中，创建或部署的Kubernetes对象模型中最小和最简单的单元。
Pod表示在群集上运行的进程。

Pod封装了应用程序的容器（或者，在某些情况下，多个容器），存储资源，唯一的网络IP以及管理容器应该如何运行的选项。
Pod表示部署单元：Kubernetes中的单个应用程序实例，可能包含单个容器或者是少量紧密耦合并共享资源的容器。

Docker是Kubernetes Pod中最常用的容器运行时，但Pods也支持其他容器运行时。

Kubernetes集群中的Pod可以以两种主要方式使用：

* 运行单个容器：`one-container-per-Pod`模型是最常见的Kubernetes用例；在这种情况下，
  您可以将Pod视为单个容器的包装，而Kubernetes直接管理Pod而不是容器。

* 运行多个需要协同工作的容器：Pod可以封装由多个共址容器组成的应用程序，这些容器紧密耦合并需要共享资源。
  这些共处一地的容器可能形成一个统一的服务单元 - 一个容器从共享卷向公众提供文件，而一个单独的“sidecar”容器刷新或更新这些文件。
  Pod将这些容器和存储资源作为单个可管理实体包装在一起。

每个Pod都用于运行给定应用程序的单个实例。如果要水平扩展应用程序（例如，运行多个实例），则应使用多个Pod，每个实例一个。
在Kubernetes中，这通常被称为复制。复制Pod通常由称为Controller的抽象创建和管理。

> 有关更多信息，请参阅[Pod Overview](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/)。

---
title: Kubernetes名词解释之Service
date: 2019-07-15 08:45:58
tags: [kubernetes]
---

Kubernetes Pod是平凡的，它门会被创建，销毁（生老病死），并且他们是不可复活的。
每个Pod都有自己的IP地址，但是在部署中，一段时间内运行的Pod集可能与稍后运行该应用程序的Pod集不同。

这会导致一个问题：如果某些Pod（称为『后端』）为集群内的其他Pod（称为『前端』）提供功能，
前端如何找出并跟踪要连接的IP地址，以便前端可以使用工作负载的后端部分？

答案是：Service

在Kubernetes中，Service是一个抽象，它定义了一组逻辑Pod和一个访问它们的策略（有时这种模式称为微服务）。
服务所针对的Pod集合通常由选择器确定。

例如，一个运行3个副本的无状态图像处理后端。那些复制品是可替代的 - 前端并不关心它们使用哪个后端。
虽然组成后端集的实际Pod可能会发生变化，但前端客户端不应该知道这一点，也不需要自己跟踪后端集。
服务抽象地实现了这种解耦。

> 有关更多信息，请参阅[Service](https://kubernetes.io/docs/concepts/services-networking/service/)。

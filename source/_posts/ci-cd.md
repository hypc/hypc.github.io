---
title: 搭建CI/CD平台
date: 2020-05-25 10:11:41
tags: [ci/cd]
---

`CI/CD`是一种通过在应用开发阶段引入自动化来频繁向客户交付应用的方法。`CI/CD`的核心概念是持续集成、持续交付和持续部署。

## 方案一

### 软件

* [Gogs][]: 是一个Git服务。
* [Jenkins][]: 是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。
* [Redmine][]: 是一个开源的、基于Web的项目管理和缺陷跟踪工具。
* [Kubernetes][]: 是一个开源平台，用于跨主机群集自动部署，扩展和操作应用程序容器，提供以容器为中心的基础架构。
* [Slack][]: Slack 是聊天群组 + 大规模工具集成 + 文件整合 + 统一搜索。
  Slack 已经整合了电子邮件、短信、Google Drives、Twitter、Trello、Asana、GitHub 等 65 种工具和服务，把可以把各种碎片化的企业沟通和协作集中到一起。

<!--more-->

### 流程图如下

![](/images/ci-cd-1.png)

### Gogs & Jenkins 集成

在Jenkins中安装[Generic Webhook Trigger][]插件，然后在Gogs中配置Webhook。

[Generic Webhook Trigger]: https://wiki.jenkins.io/display/JENKINS/Generic+Webhook+Trigger+Plugin

### Jenkins & Redmine 集成

Jenkins构建完成后，直接调用`Redmine API`接口，将对应的构建结果写入Redmine服务。

### Jenkins & Kubernetes 集成

Jenkins节点上调用`Kebuctl`命令部署服务。

## 方案二

使用`Drone`替换`Jenkins`，自动化流程基本类似，这里不再赘述。

* [Drone][]: 是一种基于容器技术的持续交付系统。


## 参考文章

* [CI/CD是什么？如何理解持续集成、持续交付和持续部署](https://www.redhat.com/zh/topics/devops/what-is-ci-cd)

[Gogs]: https://gogs.io/
[Drone]: https://drone.io/
[Jenkins]: https://www.jenkins.io/
[Redmine]: https://www.redmine.org/
[Kubernetes]: https://kubernetes.io/
[Slack]: https://slack.com/

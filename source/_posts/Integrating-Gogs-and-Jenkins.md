---
title: Integrating Gogs and Jenkins
date: 2019-03-01 08:39:04
tags: [gogs, jenkins]
---

* [Gogs][]: 是一个Git服务。
* [Jenkins][]: 是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。
* [Generic Webhook Trigger][]: Jenkins插件。

## 准备工作

### 安装Gogs

参考我的另一篇文章{% post_link docker-gogs "Deploying Gogs in Docker" %}。

### 安装Jenkins

参考我的另一篇文章{% post_link docker-jenkins "Deploying Jenkins in Docker" %}。

安装完Jenkins之后，`系统管理 -> 插件管理 -> 可选插件`，安装Generic Webhook Trigger插件。

<!--more-->

## 在Gogs中配置WebHook

![](/images/Integrating-Gogs-and-Jenkins-01.png)
![](/images/Integrating-Gogs-and-Jenkins-02.png)

## 在Jenkins中创建任务

![](/images/Integrating-Gogs-and-Jenkins-03.png)
![](/images/Integrating-Gogs-and-Jenkins-04.png)
![](/images/Integrating-Gogs-and-Jenkins-05.png)

## 测试

![](/images/Integrating-Gogs-and-Jenkins-06.png)


[Gogs]: https://gogs.io/
[Jenkins]: https://jenkins.io/
[Generic Webhook Trigger]: https://plugins.jenkins.io/generic-webhook-trigger

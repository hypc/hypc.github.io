---
title: Integrating Gogs and Redmine
date: 2019-03-05 14:58:52
tags: [gogs, redmine]
---

* [Gogs][]: 是一个Git服务。
* [Redmine][]: 是一个开源的、基于Web的项目管理和缺陷跟踪工具。

## 准备工作

### 安装Gogs

参考我的另一篇文章{% post_link docker-gogs "Deploying Gogs in Docker" %}。

### 安装Redmine

参考我的另一篇文章{% post_link docker-redmine "Deploying Redmine in Docker" %}。

<!--more-->

## 配置Gogs

将Git项目的问题集管理配置为Redmine。

![](/images/Integrating-Gogs-and-Redmine-01.png)

## 配置Redmine

`管理 -> 配置 -> 版本库`：

![](/images/Integrating-Gogs-and-Redmine-02.png)

然后在项目配置中添加版本库：

![](/images/Integrating-Gogs-and-Redmine-03.png)


[Gogs]: https://gogs.io/
[Redmine]: https://www.redmine.org/

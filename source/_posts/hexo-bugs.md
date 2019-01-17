---
title: Hexo问题集
date: 2019-01-16 09:33:10
tags: [hexo]
---

今天在使用hexo时遇到一个问题，特在此记录一下。

> 在使用`local_search`时，一直处于loading状态

有段时间发现hexo的search插件一直处于loading状态，
后来发现有一次提交的文件中带了一个不可见的特殊字符，导致search插件故障。
将该字符删除即可恢复正常。

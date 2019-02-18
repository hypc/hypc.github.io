---
title: PyCharm：Robot Framework运行环境搭建
date: 2019-02-18 09:23:00
tags: [pycharm, robotframework]
---

首先安装`IntelliBot`插件，然后配置`External Tools`：

* name: `Robot Run TestSuite`
* Group: `Robot External Tools`
* Program: `$JDKPath$`
* Parameters: `-m robot -d results --pythonpath . -v VERSION:d $FilePathRelativeToProjectRoot$`
* Working directory: `$ProjectFileDir$`

![](/images/pycharm-robot-1.png)

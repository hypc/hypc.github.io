---
title: 在PyCharm中调试Celery
date: 2020-04-02 14:52:49
tags: [python, pycharm, celery]
---

直接打开`Run/Debug Configurations`编辑器，添加一个Python配置，然后按下图填写即可：

![](/images/pycharm-celery-1.png)

<!--more-->

当然，也可按照下面方式配置，此时需要填写celery的绝对地址：

![](/images/pycharm-celery-2.png)

**举一反三**：当我们调试一些其他命令、脚本的时候可采用同样的方式进行配置，甚至是我们自己编写的脚本。

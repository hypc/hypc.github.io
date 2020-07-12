---
title: Docker问题集
date: 2019-02-22 11:27:23
tags: [docker]
---

> OCI runtime create failed: container_linux.go:348: starting container process caused "****": unknown

执行`docker run -dit ***`时遇到这个问题，现在还未找到问题原因，
删除容器、删除镜像、重启docker、重启机器都未能好使，最后`重新安装docker`才恢复正常。

----

> Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on [::1]:53: dial udp [::1]:53: connect: no route to host

问题出在DNS，极有可能是没有配置DNS或者DNS失效。

---
title: CoreOS中修改docker镜像地址
date: 2018-07-15 21:25:40
tags: [coreos, docker]
---

打开文件`/run/systemd/system/docker.service`，可以看到环境变量`$DOCKER_OPTS`，
修改docker镜像地址实际上可以认为是修改`$DOCKER_OPTS`环境变量。

打开文件`/run/flannel/flannel_docker_opts.env`，在其中添加下面内容：

```
DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"
```

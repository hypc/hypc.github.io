---
title: CoreOS安装docker-compose
date: 2018-07-15 21:23:45
tags: [coreos, docker]
---

由于目录`/usr/local/bin`不可写，所以docker-compose无法安装到`/usr/local/bin`目录下；
但是`/opt/bin`目录是可写的，该目录也在`PATH`路径下，那我们可以将`docker-compose`安装到这个目录下。

```bash
mkdir -p /opt/bin
curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-`uname -s`-`uname -m` > /opt/bin/docker-compose
chmod +x /opt/bin/docker-compose
```

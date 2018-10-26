---
title: CentOS安装最新版git
date: 2018-10-26 16:01:53
tags: [centos, git]
---

```bash
#!/bin/bash

GIT_VERSION=2.19.1

yum  groupinstall -y "Development Tools"
yum install -y zlib-devel

curl -O https://mirrors.edge.kernel.org/pub/software/scm/git/git-$GIT_VERSION.tar.gz
tar xvf git-$GIT_VERSION.tar.gz
cd git-$GIT_VERSION
./configure && make && make install
```

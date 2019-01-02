---
title: 使用Helm时遇到的一些问题
date: 2018-12-31 12:44:33
tags: [kubernetes, docker]
---

> Error: Looks like "https://kubernetes-charts.storage.googleapis.com" is not a valid chart repository or cannot be reached: read tcp 10.0.2.15:51126->172.217.160.80:443: read: connection reset by peer

执行`helm init`时失败，这个问题可以通过手动指定stable存储库为阿里云的存储库来解决。
注意，要确定tiller的版本号，可以使用`helm version`命令来查看版本号。

```bash
helm init --upgrade -i registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.12.0 --stable-repo-url https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
```

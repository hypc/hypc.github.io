---
title: MacOS开启路由转发功能
date: 2018-10-26 10:55:35
tags: [osx, shell]
---

执行下面命令即可：

```bash
sudo sysctl -w net.inet.ip.forwarding=1
```

这种做法将在机器下次重启后失效，如果想要永久保存，编辑文件`/etc/sysctl.conf`，配置下面变量：

```ini
net.inet.ip.forwarding=1
```

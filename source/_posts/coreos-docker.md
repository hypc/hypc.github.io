---
title: 将coreos中的docker服务设置为自启服务
date: 2018-07-16 21:20:18
tags: [coreos, docker]
---

```bash
systemctl enable containerd.service
systemctl enable docker.socket
systemctl enable docker.service
```

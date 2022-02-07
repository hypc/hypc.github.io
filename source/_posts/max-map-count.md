---
title: 调整vm.max_map_count
date: 2020-11-20 21:07:45
tags: [linux]
---

`vm.max_map_count`表示一个进程可以拥有的VMA（虚拟内存区块）的数量。

**查看当前值**

```bash
sysctl -a | grep vm.max_map_count
```

**修改**

```bash
echo 'vm.max_map_count=655350' >> /etc/sysctl.conf
sysctl -p
```

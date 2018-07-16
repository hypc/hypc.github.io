---
title: Windows下实现端口转发
date: 2018-07-16 21:14:41
tags: [windows]
---

Windows下可以使用netsh做端口转发：

```cmd
# 将192.168.5.3的22端口转发到xxx.xxx.xxx.xxx的22端口
netsh interface portproxy add v4tov4 listenaddress=192.168.5.3 listenport=22 connectaddress=xxx.xxx.xxx.xxx connectport=22
# 查看存在的转发
netsh interface portproxy show all
# 删除IP是192.168.5.3，端口是22的转发
netsh interface portproxy delete v4tov4 listenaddress=192.168.5.3 listenport=22
```

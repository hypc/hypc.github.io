---
title: python获取本机IP、mac地址、计算机名
date: 2018-07-08 10:52:53
tags: [python]
---

```python
import uuid
import socket

def get_mac_address():
    mac=uuid.UUID(int = uuid.getnode()).hex[-12:]
    return ":".join([mac[e:e+2] for e in range(0,11,2)])

# 获取Mac地址
mymac = get_mac_address()
#获取本机电脑名
myname = socket.getfqdn(socket.gethostname())
#获取本机ip
myaddr = socket.gethostbyname(myname)

print(mymac)
print(myname)
print(myaddr)
```

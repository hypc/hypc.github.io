---
title: Python获取本机ip
date: 2018-10-27 15:54:24
tags: [python]
---

本方法采用UDP协议实现，先生成一个UDP包，将自己的ip地址放到UDP的协议头中，然后从UDP包中获取ip地址。

这个方法并不会发送数据，但是会申请一个UDP端口，经常调用会比较耗时。

```python
import socket
 
def get_host_ip():
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(('8.8.8.8', 80))
        ip = s.getsockname()[0]
    finally:
        s.close()
    return ip
```

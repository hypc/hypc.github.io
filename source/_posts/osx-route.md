---
title: OSX系统路由操作
date: 2018-07-12 13:37:40
tags: [osx, shell]
---

**查看路由表：**

```bash
netstat -nr
```

**删除路由：**

```bash
route delete 0.0.0.0        # 删除默认路由
route delete 10.0.100.0     # 删除普通路由
```

**添加路由：**

```bash
route add -net 0.0.0.0 192.168.1.1          # 添加默认路由
route add 192.168.2.0/24 192.168.1.1   # 将192.168.2网段的网关设置为192.168.1.1
```

**附：**

全球IP段查询：http://ipblock.chacuo.net/

中国最新IP段：http://ipblock.chacuo.net/down/t_txt=c_CN

```bash
cnroute(){
    command curl -s http://ipblock.chacuo.net/down/t_txt=c_CN | awk '{print $3}' | xargs -I {} sudo route add {} 192.168.1.1
}
```

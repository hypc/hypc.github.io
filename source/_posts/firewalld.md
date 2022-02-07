---
title: firewalld常见用法
date: 2020-12-12 18:59:51
tags: [shell, firewalld, linux, firewall-cmd]
---

前两天，CentOS项目宣布，CentOS8将于2021年底结束，而CentOS7将在其生命周期结束后停止维护。
于是，我又将个人VPS装回CentOS7。

下面介绍firewalld的常见用法：

```bash
# 显示所有配置
firewall-cmd --list-all

# 临时打开443/TCP端口
firewall-cmd --zone=public --add-port=443/tcp
# 永久打开443/TCP端口
firewall-cmd --permanent --zone=public --add-port=443/tcp
# 移除443端口
firewall-cmd --zone=public --remove-port=443/tcp

# 临时添加http服务
firewall-cmd --zone=public --add-service=http
# 永久添加http服务
firewall-cmd --permanent --zone=public --add-service=http
# 移除http服务
firewall-cmd --zone=public --remove-service=http

# 在不改变状态的条件下重新加载防火墙
firewall-cmd --reload
```

更多用法参见：[firewall-cmd][]。

[firewall-cmd]: https://github.com/jaywcjlove/linux-command/blob/master/command/firewall-cmd.md

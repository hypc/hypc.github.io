---
title: SSH免密登录
date: 2018-07-08 11:04:24
tags: [shell, ssh]
---

**客户端操作**

```bash
# 客户端生成公钥私钥
ssh-keygen -t rsa
# 将公钥传送到服务器上
scp ~/.ssh/id_rsa.pub root@192.168.1.2:~/aaa.pub
```

**服务端操作**

```bash
# 将公钥添加到 authorized_keys 文件后面
cat aaa.pub >> ~/.ssh/authorized_keys
```

## SELinux引起的ssh公钥认证失败问题

`sshd`配置正确的情况下，还是无法使用`publickey`登陆，那么极有可能是被`SELinux`拦截了，
造成的原因可能是`.ssh`目录的权限错误。

```shell
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
restorecon -r -vv ~/.ssh
```

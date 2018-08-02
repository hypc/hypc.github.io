---
title: 以其他用户身份运行shell命令的几种方式
date: 2018-08-02 19:58:47
tags: [shell]
---

## 以ROOT用户身份运行

使用`sudo`命令：

```bash
sudo command
```

## 以命令所有者的身份运行

**首先以命令所有者的身份修改命令的权限**

```bash
chmod u+s command
```

**当其他用户执行`command`命令时，会以命令所有者的身份运行该命令**

```bash
# 执行命令
command
# 查看运行状态
ps aux | grep command
```

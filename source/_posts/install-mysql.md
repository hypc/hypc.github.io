---
title: Ubuntu安装MySQL
date: 2018-07-05 16:09:02
tags: [mysql, ubuntu]
---

执行以下命令安装：

```bash
apt-get update && apt-get install -y mysql-server mysql-client libmysqlclient-dev
```

**注意**：安装时候中间需要输入mysql数据库的密码，如果不想在安装过程中输入密码，可以在安装之前执行下面命令：

```bash
debconf-set-selections <<< 'mysql-server mysql-server/root_password password your_password'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password your_password'
```

---
title: 安装PostgreSQL
date: 2018-07-28 09:20:04
tags: [postgresql]
---

## Ubuntu14.04上安装

```bash
echo 'deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main' > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
apt-get update
apt-get install -y postgresql-9.4
```

### 修改为远程登录

修改文件`/etc/postgresql/9.4/main/postgresql.conf`:

```
-#listen_addresses = 'localhost'
+listen_addresses = '*'
-#password_encryption = on
+password_encryption = on
```

### 修改访问地址

修改文件`/etc/postgresql/9.4/main/pg_hba.conf`:

```
+host    all             all             10.0.0.0/8              trust
+host    all             all             172.16.0.0/12           trust
+host    all             all             192.168.0.0/16          trust
```

## CentOS7上安装

```bash
yum install http://yum.postgresql.org/9.5/redhat/rhel-7-x86_64/pgdg-redhat95-9.5-2.noarch.rpm
yum install -y postgresql95 postgresql95-server postgresql95-libs postgresql95-contrib postgresql95-devel
/usr/pgsql-9.5/bin/postgresql95-setup initdb
systemctl enable postgresql-9.5.service
systemctl start postgresql-9.5.service
```

如果需要postgis：

```bash
yum install -y epel-release
yum install -y postgis2_95 postgis2_95-client
```

### 修改为远程登录

修改文件`/var/lib/pgsql/9.4/data/postgresql.conf`:

```
-#listen_addresses = 'localhost'
+listen_addresses = '*'
-#password_encryption = on
+password_encryption = on
```

### 修改访问地址

修改文件`/var/lib/pgsql/9.4/data/pg_hba.conf`:

```
+host    all             all             10.0.0.0/8              trust
+host    all             all             172.16.0.0/12           trust
+host    all             all             192.168.0.0/16          trust
```

## DebianJessie上安装

```bash
echo 'deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main' > /etc/apt/sources.list.d/pgdg.list
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
apt-get update
apt-get install -y postgresql-9.4
```

### 修改为远程登录

修改文件`/etc/postgresql/9.4/main/postgresql.conf`:

```
-#listen_addresses = 'localhost'
+listen_addresses = '*'
-#password_encryption = on
+password_encryption = on
```

### 修改访问地址

修改文件`/etc/postgresql/9.4/main/pg_hba.conf`:

```
+host    all             all             10.0.0.0/8              trust
+host    all             all             172.16.0.0/12           trust
+host    all             all             192.168.0.0/16          trust
```

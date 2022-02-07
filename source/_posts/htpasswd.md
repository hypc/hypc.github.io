---
title: htpasswd
date: 2021-01-17 19:47:04
tags: [ssl, htpasswd]
---

[htpasswd][]命令是Apache的Web服务器内置工具，用于创建和更新储存用户名、域和用户基本认证的密码文件。

[htpasswd]: https://httpd.apache.org/docs/2.4/misc/password_encryptions.html

较常用的是`MD5`算法，可以使用下面命令生成密钥串：

```bash
# 使用htpasswd命令
$ htpasswd -nbm myName myPassword
myName:$apr1$izvq8Tz5$vBk6U1mybuKXyth86sDfc/

# 使用openssl命令
$ openssl passwd -apr1 myPassword
$apr1$kRZXMOsR$qRNQdigt90XJkWbyzftwa1
```

---
title: 让sudo操作不输入密码
date: 2018-07-07 10:10:54
tags: [shell]
---

假设你想让用户名为`test`的用户可以在使用sudo的时候不输入密码，
你需要修改文件`/etc/sudoers.d/test`（没有则创建），其中test为你的用户名。文件内容为：

```
test ALL=(ALL) NOPASSWD:ALL
```

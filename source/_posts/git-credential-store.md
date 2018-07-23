---
title: Git保存帐号密码
date: 2018-07-23 21:29:18
tags: [git]
---

```bash
# 缓存15分钟，也就是15分钟内不用重复输入帐号密码
git config credential.helper cache
git config --global credential.helper cache
# 可以自定义缓存时间，以下是缓存1小时
git config credential.helper 'cache --timeout=3600'
git config --global credential.helper 'cache --timeout=3600'
# 也可以永久缓存
git config credential.helper store
git config --global credential.helper store
```

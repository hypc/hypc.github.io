---
title: Git Pull部分文件
date: 2018-07-05 16:19:25
tags: [git]
---

```bash
git init
git remote add -f origin https://xxx/git/xxx.git
git config core.sparsecheckout true
echo "docs" >> .git/info/sparse-checkout
git pull origin master
git branch --set-upstream-to=origin/master master
```

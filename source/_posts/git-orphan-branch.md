---
title: Git：创建一个空分支
date: 2019-03-12 16:58:49
tags: [git]
---

有时候，我们希望创建一个新的分支，而又不想继承任何提交，没有一个父节点，是一个完全新的干净的分支，
例如有时候我们希望创建一个分支用来放置文档。

这是后可以使用`--orphan`参数创建分支：

```bash
git checkout --orphan doc
```

这时候，当前分支会有前面那个分支的所有文件，可以执行下面命令删除所有文件：

```bash
git rm -rf .
```

然后提交分支：

```bash
touch README.md
git add .
git commit -m 'new branch for documentation'
```

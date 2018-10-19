---
title: Git简明教程
date: 2018-10-12 08:37:17
tags: [git]
---

## Git常用命令

### git help

Git帮助命令，用来查看Git帮助，直接使用`git help`可以查看git所有相关的子命令，
使用`git help <subcommand>`可以查看对应子命令的详细内容。

```bash
git help        # 查看git帮助
git help init   # 查看git初始化命令帮助
git help commit # 查看git代码提交帮助
```

### git init

Git初始化命令，直接在一个空目录下执行命令`git init`即可，
当然，也可以在一个不是git项目的目录下执行。

### git config

Git配置命令，用来配置一些git常用的配置参数，例如用户名等。

git配置一般分为两种，一种是当前项目的配置，一种是全局的配置。
如果两个位置都有同一个配置，当前项目的配置覆盖全局的配置。

```bash
git config user.name                # 查看用户名设置
git config user.name <your_name>    # 设置用户名
git config --global core.editor vim # 全局设置git命令编辑器为vim
```

### git add

将待提交的文件缓存到暂存区。

```bash
git add <filename>  # 将文件filename提交到暂存区
git add <folder>    # 将目录folder下的改动过的文件都提交到暂存区
git add .           # 将项目所有改动的文件提交到暂存区
```

### git commit

将暂存区中的文件提交。

```bash
git commit -m '<reason>'    # 提交，并解释提交的理由
```

<!--more-->

### git status

查看当前项目的状态，一般会有4中状态，新建状态、更新状态、已缓存状态、冲突状态。

```bash
git status
```

### git diff

查看代码的差别。

```bash
git diff    # 查看当前代码与最后一次提交的代码的差别
git diff branch1 branch2    # 查看branch1分支与branch2分支的差别
```

### git log

查看项目提交的历史记录。

```bash
git log
git log -5  # 查看最近5次提交，也可以改成其他数字
```

### git tag

Git标签命令。

```bash
git tag         # 列出已有的标签
git tag <tagname>       # 新建普通标签
git tag -a <tagname> -m 'tagmsg'    # 新建一个带tagmsg消息的tagname标签
```

### git clone

Git克隆代码，将远程服务器上的代码克隆到本地。

```bash
git clone https://github.com/xxx/project.git
git clone https://github.com/xxx/project.git local_project  # 将远程服务器上的project项目克隆到本地，并改名local_project
```

### git pull

当远程服务器上的代码比本地代码新时，可以使用`git pull`将远程服务器上的代码更新到本地。

```bash
git pull
```

### git push

当需要将本地已提交的代码提交到远程服务器上时，可以使用`git push`命令将本地代码提交到远程服务器上。

```bash
git push
```

### git merge

Git合并代码命令。

### git branch

Git分支命令。

```bash
git branch      # 列出当前已有的分支
git branch <branch_name>    # 新建一个分支
```

### git stash

`git stash`命令可以将当前未提交的代码保存起来。

```bash
git stash       # 将未提交的代码保存起来
git stash pop   # 将最后一次保存的代码pop出来
git stash list  # 查看所有保存的记录
git stash drop <stash>  # 删除某个保存
git stash clear # 清除所有保存
```

## Git其他命令

### git reset

### git rebase

### git submodule

### git rm

### git grep

### git fetch

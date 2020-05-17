---
title: 在Linux上如何清除Cache和Swap
date: 2020-02-07 10:35:28
tags: [linux, shell]
---

在Linux系统中有先进的缓存机制，会针对dentry（用于VFS，加速文件路径名到inode的转换）、
Buffer Cache（针对磁盘块的读写）和Page Cache（针对文件inode的读写）进行缓存操作，有效缩短 I/O系统调用（比如read、write）的时间。
但当进行了大量文件操作之后，缓存会把内存资源基本用光，导致系统缓慢，使用swap空间，影响了性能，这时就需要清理缓存了。

## 如何清除Cache

在清除Cache之前，先执行[sync][]命令将缓冲区的数据写入磁盘，
然后通过向文件`/proc/sys/vm/drop_caches`写入值`1`、`2`或`3`的方式来清除Cache：

*注意：文件`/proc/sys/vm/drop_caches`只有在进行写操作时才会触发清除cache操作*

```bash
# 1. 仅清除页面缓存（PageCache）
sync; echo 1 > /proc/sys/vm/drop_caches
# 2. 清除目录项和inode
sync; echo 2 > /proc/sys/vm/drop_caches
# 3. 清除页面缓存，目录项和inode
sync; echo 3 > /proc/sys/vm/drop_caches
```

第一条命令是最安全的，它只会清除页面缓存，一般情况下，我们只需要执行第一条命令即可。
在生产环境中，不建议执行第三条命令，除非你明确知道你需要执行什么。

[sync]: https://man.linuxde.net/sync

<!--more-->

## 如何清除Swap

可以通过开关Swap的方式来清除Swap：

```bash
swapoff -a && swapon -a
```

## 查看内存使用情况

使用[free][]命令来查看内存使用情况：

```
# free -h
              total        used        free      shared  buff/cache   available
Mem:          7.5Gi       1.5Gi       5.3Gi       297Mi       704Mi       5.4Gi
Swap:         7.8Gi          0B       7.8Gi
```

[free]: https://man.linuxde.net/free

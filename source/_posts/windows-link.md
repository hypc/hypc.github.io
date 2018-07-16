---
title: Windows下创建软连接
date: 2018-07-16 21:11:16
tags: [windows]
---

```cmd
> mklink
MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件符号链接。
        /H      创建硬链接而非符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径(相对或绝对)。

> mklink D:\filelink.txt E:\file.txt
> mklink /D D:\folderlink E:\folder
```

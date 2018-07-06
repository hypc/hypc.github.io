---
title: GVim UTF-8乱码
date: 2018-07-06 15:39:53
tags: [vim]
---

打开文件`.vimrc`或`_vimrc`，在开头处加上：

```bash
let $LANG="zh_CN.UTF-8"
set fileencodings=utf-8,chinese,latin-1
set termencoding=utf8
set encoding=utf-8
```

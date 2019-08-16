---
title: tree命令显示中文问题
date: 2019-08-04 13:42:06
tags: [shell]
---

执行`tree`指令，它会列出指定目录下的所有文件，包括子目录里的文件。

当遇到文件名（或目录名）中包含中文时，会出现乱码：

![](/images/tree-bug-cn-1.png)

这时候可以添加参数`-N`，执行命令`tree -N`：

![](/images/tree-bug-cn-2.png)

常用别名：

```bash
alias tree='tree -CNF'
alias treel='tree -alF'
```

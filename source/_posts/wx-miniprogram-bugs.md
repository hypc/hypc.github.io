---
title: 微信小程序问题集
date: 2019-02-15 10:00:46
tags: [nodejs]
---

> VM175:4 /bin/sh: npm: command not found

在MacOS系统下，编译小程序时可能会遇到这个问题。由于我是使用[`nvm`][nvm]管理多个nodejs版本，
而微信开发者工具并不会切换nodejs版本，导致找不到对应的nodejs版本。

这个问题解决办法是，`项目设置 -> 启用自定义处理命令`，配置下面命令：

```bash
source ~/.profile && nvm use 8 && npm run compile
```

[nvm]: https://github.com/creationix/nvm

---
title: nvm
date: 2018-07-05 13:21:35
tags: [nvm, nodejs]
---

根据[nvm][]进行安装，安装之后会在你的profile（`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`）文件中添加以下两行：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm
```

然后在后面添加一行：

```ini
NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
NVM_IOJS_ORG_MIRROR=https://npm.taobao.org/mirrors/iojs
```

[nvm]: https://github.com/creationix/nvm

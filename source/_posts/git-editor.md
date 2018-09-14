---
title: 修改Git默认编辑器
date: 2018-09-11 16:57:44
tags: [git]
---

1. 使用`git config`设置：

    ```bash
    git config --global core.editor vim     # 全局设置
    git config core.editor vim              # 当前项目设置
    ```

2. 使用环境变量`GIT_EDITOR`：

    ```bash
    export GIT_EDITOR=vim
    ```

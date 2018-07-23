---
title: 修改Git中已提交的用户名和邮箱
date: 2018-07-23 21:20:45
tags: [git]
---

为修改已经存在的commit中的用户名和邮箱，必须重写整个git repo的提交历史。

> 警告：这种行为对提交历史具有破坏性，在无必要的情况下，不建议对其修改。
>
> 注意：完成重写后，任何fork或clone的人必须重新获取重写后的历史并把所有本地修改`rebase`入重写后的历史中。

1. 复制以下脚本，并根据自己的需要修改以下变量：

    * `OLD_EMAIL`: 要修改的邮箱
    * `CORRECT_NAME`: 修改之后的用户名
    * `CORRECT_EMAIL`: 修改之后的邮箱

    脚本：

    ```bash
    #!/bin/sh

    OLD_EMAIL="your-old-email@example.com" CORRECT_NAME="Your Correct Name" CORRECT_EMAIL="your-correct-email@example.com" git filter-branch --env-filter '
    if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]; then
        export GIT_COMMITTER_NAME="$CORRECT_NAME"
        export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]; then
        export GIT_AUTHOR_NAME="$CORRECT_NAME"
        export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
    fi
    ' --tag-name-filter cat -- --branches --tags
    ```

2. 执行脚本

3. 常看历史有没有错误

4. 将重写后的历史提交到Git远程仓库中：`git push --force --tags origin 'refs/heads/*'`

> 建议以上操作在一个新clone的仓库中进行，这样的话可以减少一些误操作。

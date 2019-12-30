---
title: Git代码统计
date: 2019-12-11 16:38:26
tags: [git, shell]
---

```bash
#!/bin/bash

git-stats(){
    local since=1970-01-01
    local until=3000-01-01
    if [[ -n "$1" ]]; then
        since=$1
    fi
    if [[ -n "$2" ]]; then
        until=$2
    fi

    git log --format='%aN' --since=${since} --until=${until} | sort -u | while read author; do
        echo -n "${author},"
        git log --author="${author}" --pretty=tformat: --numstat --since=${since} --until=${until} \
            | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "%s,%s,%s\n", add, subs, loc }' -
    done
}
```

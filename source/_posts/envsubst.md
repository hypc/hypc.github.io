---
title: envsubst
date: 2018-07-14 10:19:25
tags: [shell]
---

```
envsubst [option] [shell-format]
```

详见：[https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html][1]

**示例**

```bash
$ env
a=1
b=2
c=3
...
$ cat a.template
a=$a
b=$b
c=$c
$ envsubst < a.template
a=1
b=2
c=3
$ envsubst '$$a $$b' < a.template
a=1
b=2
c=$c
```

[1]: https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html

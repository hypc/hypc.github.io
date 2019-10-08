---
title: virtualenv问题集
date: 2019-10-08 15:31:11
tags: [python]
---

> OSError: Command /home/username/xxx/bin/python3 - setuptools pkg_resources pip wheel failed with error code 2

今天在使用virtualenv创建虚拟环境的时候报错，
一通查找之后发现是`setuptools`、`virtualenv`版本太旧的问题：

```bash
pip install -U setuptools virtualenv
```

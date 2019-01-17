---
title: Python获取当前工作目录
date: 2019-01-09 10:28:51
tags: [python]
---

使用`os`模块获取当前工作目录：

```python
import os

print(os.getcwd())                  # 获得当前工作目录
print(os.path.abspath('.'))         # 获得当前工作目录
print(os.path.abspath('..'))        # 获得当前工作目录的父目录
print(os.path.abspath(os.curdir))   # 获得当前工作目录
```

---
title: Python：partial函数
date: 2020-11-28 21:26:43
tags: [python]
---

[partial][]: New function with partial application of the given arguments and keywords.

简单来说，便是为一个函数预设初始参数，并创建一个新的函数。大致相当于：

```python
def partial(func, /, *args, **keywords):
    def newfunc(*fargs, **fkeywords):
        newkeywords = {**keywords, **fkeywords}
        return func(*args, *fargs, **newkeywords)
    newfunc.func = func
    newfunc.args = args
    newfunc.keywords = keywords
    return newfunc
```

示例如下：

```python
>>> from functools import partial
>>> basetwo = partial(int, base=2)
>>> basetwo.__doc__ = 'Convert base 2 string to an int.'
>>> basetwo('10010')
18
```

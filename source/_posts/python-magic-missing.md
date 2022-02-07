---
title: Python 魔法函数 - __missing__
date: 2021-12-27 11:16:23
tags: [python]
---

魔法函数 [object.__missing__][__missing__]，官方原文描述如下：

> Called by [dict.__getitem__()][__getitem__] to implement `self[key]` for dict subclasses when key is not in the dictionary.

[__missing__]: https://docs.python.org/3.8/reference/datamodel.html#object.__missing__
[__getitem__]: https://docs.python.org/3.8/reference/datamodel.html#object.__getitem__

翻译过来大概意思是：当语句 `self[key]` 中的 `key` 不在字典中的时候被调用。

通常情况下，我们从 dict 中获取一个不存在的 key 时会抛出 KeyError 异常：

```python
>>> d = {}
>>> d['a']
Traceback (most recent call last):
  File "/***/code.py", line 90, in runcode
    exec(code, self.locals)
  File "<input>", line 1, in <module>
KeyError: 'a'
```

如果不想抛出异常，那么大概率会使用 `defaultdict`：

```python
>>> from collections import defaultdict
>>> d = defaultdict(lambda: 'default value')
>>> d['a']
'default value'
```

使用 `help(dict)` 可以发现，dict 描述中虽然有许多魔法函数，但是却没有 `__missing__`。
而使用 `help(defaultdict)` 查看，可以发现：

```python
class defaultdict(builtins.dict)
 |  defaultdict(default_factory[, ...]) --> dict with default factory
 |  ...
 |  __missing__(...)
 |      __missing__(key) # Called by __getitem__ for missing key; pseudo-code:
 |      if self.default_factory is None: raise KeyError((key,))
 |      self[key] = value = self.default_factory()
 |      return value
 |  ...
```

---
title: Python魔法函数：属性访问
date: 2019-09-23 13:32:02
tags: [python]
---

Python有以下关于属性访问的魔法方法：

* `__getattr__(self, item)`: 访问不存在的属性时调用
* `__getattribute__(self, item)`: 访问属性时调用（先调用该方法，查看是否存在该属性，若不存在，则继续调用方法`__getattr__`）
* `__setattr__(self, key, value)`: 设置对象属性时调用
* `__delattr__(self, item)`: 删除属性时调用

```python
class Test:
    def __getattr__(self, item):
        print('__getattr__')

    def __getattribute__(self, item):
        print('__getattribute__')
        return super().__getattribute__(item)   # or: object.__getattribute__(self, item)

    def __setattr__(self, key, value):
        print('__setattr__')
        super().__setattr__(key, value)     # or: object.__setattr__(self, key, value)

    def __delattr__(self, item):
        print('__delattr__')
        super().__delattr__(item)       # or: object.__delattr__(self, item)

>>> test = Test()
>>> test.a
__getattribute__
__getattr__
>>> test.a = 1
__setattr__
>>> test.b
__getattribute__
1
```

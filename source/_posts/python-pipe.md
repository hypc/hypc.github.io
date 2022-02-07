---
title: Python 管道 Pipe
date: 2022-01-13 17:17:59
tags: [python]
---

## 入门

**安装**

```bash
pip install pipe
```

**examples**

```python
>>> import * from pipe
>>> sum(range(100) | select(lambda x: x ** 2) | where(lambda x: x < 100))
285
>>> sum([1, 2, 3, 4] | where(lambda x: x % 2 == 0))
6
>>> sum([1, [2, 3], 4] | traverse)
10
```

<!--more-->

## 源码解读

阅读源码后，发现 pipe 关键代码是 `Pipe` 这个装饰器：

```python
class Pipe:
    """
    Represent a Pipeable Element :
    Described as :
    first = Pipe(lambda iterable: next(iter(iterable)))
    and used as :
    print [1, 2, 3] | first
    printing 1
    Or represent a Pipeable Function :
    It's a function returning a Pipe
    Described as :
    select = Pipe(lambda iterable, pred: (pred(x) for x in iterable))
    and used as :
    print [1, 2, 3] | select(lambda x: x * 2)
    # 2, 4, 6
    """

    def __init__(self, function):
        self.function = function
        functools.update_wrapper(self, function)

    def __ror__(self, other):
        return self.function(other)

    def __call__(self, *args, **kwargs):
        return Pipe(lambda x: self.function(x, *args, **kwargs))
```

`Pipe` 这个装饰器重写了魔法函数 `__ror__`，实现了按位或运算符 `|`。
这样，我们可以使用管道符 `|` 将多个操作连接起来，就如同上面的示例。

下面是官方提供的几个典型的操作：

```python
@Pipe
def dedup(iterable, key=lambda x: x):
    """Only yield unique items. Use a set to keep track of duplicate data."""
    seen = set()
    for item in iterable:
        dupkey = key(item)
        if dupkey not in seen:
            seen.add(dupkey)
            yield item

@Pipe
def select(iterable, selector):
    return builtins.map(selector, iterable)

@Pipe
def where(iterable, predicate):
    return (x for x in iterable if (predicate(x)))

@Pipe
def groupby(iterable, keyfunc):
    return itertools.groupby(sorted(iterable, key=keyfunc), keyfunc)

@Pipe
def sort(iterable, **kwargs):
    return sorted(iterable, **kwargs)

@Pipe
def reverse(iterable):
    return reversed(iterable)
```

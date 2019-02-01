---
title: Python的with语句块
date: 2019-02-01 09:02:02
tags: [python]
---

WITH语句块用于使用上下文管理器定义的方法。语法结构如下：

```python
with_stmt ::=  "with" with_item ("," with_item)* ":" suite
with_item ::=  expression ["as" target]
```

我们经常会用到文件读写：

```python
f = open("a.txt", "r")
f.read()
f.close()
```

使用with语句块可以这么写：

```python
with open("a.txt", "r") as f:
    f.read()
```

<!--more-->

也可以同时打开多个文件：

```python
with open("a.txt", "r") as a, open("b.txt", "r") as b:
    a.read()
    b.read()
```

我们还可以定义自己的类使用with语句，只需要实现`__enter__`、`__exit__`这两个函数，
详见[With Statement Context Managers][context-managers]：

```python
class A(object):
    def __enter__(self):
        print("----enter----")

    def __exit__(self, exc_type, exc_value, traceback):
        print("----exit----")

with A() as a:
    print("----with----")

# >>> ----enter----
# >>> ----with----
# >>> ----exit----
```

除了上面那种方法之外，还可以使用[contextlib.contextmanager][contextlib.contextmanager]装饰器：

```python
from contextlib import contextmanager

@contextmanager
def managed_resource(*args, **kwds):
    # Code to acquire resource, e.g.:
    resource = acquire_resource(*args, **kwds)
    try:
        yield resource
    finally:
        # Code to release resource, e.g.:
        release_resource(resource)

with managed_resource(timeout=3600) as resource:
    pass
```

注意，被装饰的方法必须返回一个可迭代的对象，并且只能有一个值，这个值作为`with`语句块的`as`语句的内容，
另外，在`yield`之后需要释放对应的资源。所以，推荐的写法如上，被装饰的方法前半部分获取相应的资源，
然后`yield`出来，后半部分释放对应的资源。推荐`try...except...finally...`写法。


[context-managers]: https://docs.python.org/3.7/reference/datamodel.html#context-managers
[contextlib.contextmanager]: https://docs.python.org/3.7/library/contextlib.html#contextlib.contextmanager

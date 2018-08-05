---
title: 【转载】Python中__all__的作用
date: 2018-08-05 19:47:50
tags: [python]
---

它是一个string元素组成的list变量，定义了当你使用`from <module> import *`
导入某个模块的时候能导出的符号（这里代表变量，函数，类等）。

举个例子，下面的代码在 foo.py 中，明确的导出了符号 bar, baz：

```python
__all__ = ['bar', 'baz']

waz = 5
bar = 10
def baz():
    return 'baz'
```

导入实现如下：

```python
from foo import *

print bar
print baz

# The following will trigger an exception, as "waz" is not exported by the module
# 下面的代码就会抛出异常，因为 "waz"并没有从模块中导出，因为 __all__ 没有定义
print waz
```

如果把`foo.py`中`__all__`给注释掉，那么上面的代码执行起来就不会有问题，
`import *`默认的行为是从给定的命名空间导出所有的符号（当然下划线开头的私有变量除外）。

参考：https://docs.python.org/3.5/tutorial/modules.html#importing-from-a-package

**注意：**需要注意的是`__all__`只影响到了`from <module> import *`这种导入方式，
对于`from <module> import <member>`导入方式并没有影响，仍然可以从外部导入。


转载自：https://stackoverflow.com/questions/44834/can-someone-explain-all-in-python

---
title: Python元类
date: 2020-12-26 19:38:49
tags: [python]
---

在python官方文档有一句话：

> By default, classes are constructed using type(). The class body is executed in a new namespace and the class name is bound locally to the result of type(name, bases, namespace).

这句话的大概意思是说：在默认情况下，类都是由`type`构建而来。

```python
A = type('A', (object,), {})

class A(object):
    pass

class A(object, metaclass=type):
    pass
```

以上三种创建类`A`的方式等价。

<!--more-->

那么我们是否可以通过继承`type`的方式实现自己的`mytype`？答案是可以：

```python
class mytype(type):
    pass
```

然后：

```python
B = mytype('B', (object,), {})

class BB(object, metaclass=mytype):
    pass
```

我们将`mytype`改造一下：

```python
class mytype(type):
    def __init__(cls, name, bases, namespace):
        super().__init__(name, bases, namespace)
        setattr(cls, 'hello', lambda self: 'hello world!')
```

然后：

```python
>>> print(B().hello())
hello world!
>>> print(B.__dict__)
{'__module__': 'test', '__dict__': <attribute '__dict__' of 'B' objects>, '__weakref__': <attribute '__weakref__' of 'B' objects>, '__doc__': None, 'hello': <function mytype.__init__.<locals>.<lambda> at 0x108d74b80>}
>>> print(BB().hello())
hello world!
>>> print(BB.__dict__)
{'__module__': 'test', '__dict__': <attribute '__dict__' of 'BB' objects>, '__weakref__': <attribute '__weakref__' of 'BB' objects>, '__doc__': None, 'hello': <function mytype.__init__.<locals>.<lambda> at 0x108d74d30>}
```

将`mytype`再次改造：

```python
class mytype(type):
    def __new__(mcls, name, bases, namespace):
        cls = super().__new__(mcls, name, bases, namespace)
        setattr(cls, 'hello', lambda self: 'hello world!')
        return cls
```

再次执行以下代码，可以发现结果几乎相同：

```python
>>> print(B().hello())
hello world!
>>> print(B.__dict__)
{'__module__': 'test', '__dict__': <attribute '__dict__' of 'B' objects>, '__weakref__': <attribute '__weakref__' of 'B' objects>, '__doc__': None, 'hello': <function mytype.__init__.<locals>.<lambda> at 0x104c6baf0>}
>>> print(BB().hello())
hello world!
>>> print(BB.__dict__)
{'__module__': 'test', '__dict__': <attribute '__dict__' of 'BB' objects>, '__weakref__': <attribute '__weakref__' of 'BB' objects>, '__doc__': None, 'hello': <function mytype.__init__.<locals>.<lambda> at 0x104c6bca0>}
```

最后，再提一句，元类的实现上，倾向于使用`__new__`而不是`__init__`。`__new__`与`__init__`的区别，大家可以自行去了解。

---
title: Python：使用type动态构建类
date: 2019-10-30 09:16:14
tags: [python]
---

Python内建的`type`自带3个参数，有两种用法：

1. `type(object) -> the object's type`
2. `type(name, bases, dict) -> a new type`

第一种用法就不说了，今天主要说一下第二种用法。

## `type(name, bases, dict)`

* `name`: 字符串，制定要构造类的名字，赋给新对象的`__name__`属性
* `bases`: 一个tuple，指定新类型的所有基类，赋给新对象的`__bases__`属性
* `dict`: 字典类型，作为新类的名字空间，赋给新对象的`__dict__`属性

<!--more-->

我们通常创建一个类的做法是：

```python
class A(object):
    pass
```

那么通过下面代码也能达到同样的效果：

```python
A = type('A', (object,), {})
```

通过使用类属性的方式我们可以添加成员属性或成员函数：

```python
A.bar = 'bar'

def foo(self):
    pass

A.foo = foo
```

使用`type`可以这么做：

```python
A = type('A', (object,), {'bar': 'bar', 'foo': foo})
```

## 使用场景

1. 之前做过一个物联网项目，里面有一个设备管理模块，除了一些通用属性外，不同种类的设备属性基本上都不一样。
   这里我们的做法是在后台创建一种新的设备，配置好这类新设备的属性及其值域范围。
   然后使用`type`动态创建设备子类来管理设备。这样就不用每次添加一种新设备都需要发一次版本问题。

2. 一般电子商城中的商品管理也有类似的问题，不同种类的商品属性差异化更加严重。
   同样，我们可以在后台配置好商品种类，然后使用`type`动态创建商品子类来管理商品。

---
title: 关于Django字段类型的问题
date: 2019-03-07 09:14:13
tags: [python, django]
---

今天有个同事遇到一个问题，创建Model对象的时候，字段类型与预期的不一致：

```python
class DemoModel(models.Model):
    demoid = models.UUIDField(primary_key=True, default=uuid.uuid1)
    str_field = models.CharField(max_length=100)
```

```python
>>> from app1.models import DemoModel
>>> m = DemoModel.objects.create(**{'str_field': 1})
>>> type(m.str_field)       ## 这时候字段类型是int而不是预想中的str
<class 'int'>
>>> m.str_field
1
>>> m = DemoModel.objects.first()
>>> type(m.str_field)       ## 此时字段类型是str
<class 'str'>
>>> m.str_field
'1'
```

<!--more-->

**问题分析**

查看源码发现`create`函数是先new了一个model对象，然后调用model的`save`方法：

```python
class QuerySet(object):
    ...
    def create(self, **kwargs):
        """
        Creates a new object with the given kwargs, saving it to the database
        and returning the created object.
        """
        obj = self.model(**kwargs)
        self._for_write = True
        obj.save(force_insert=True, using=self.db)
        return obj
    ...
```

接着查看model的`__init__`函数，发现这里只是简单的设置字段值：

```python
class Model(six.with_metaclass(ModelBase)):

    def __init__(self, *args, **kwargs):
        signals.pre_init.send(sender=self.__class__, args=args, kwargs=kwargs)

        # Set up the storage for instance state
        self._state = ModelState()

        # There is a rather weird disparity here; if kwargs, it's set, then args
        # overrides it. It should be one or the other; don't duplicate the work
        # The reason for the kwargs check is that standard iterator passes in by
        # args, and instantiation for iteration is 33% faster.
        args_len = len(args)
        if args_len > len(self._meta.concrete_fields):
            # Daft, but matches old exception sans the err msg.
            raise IndexError("Number of args exceeds number of fields")

        if not kwargs:
            fields_iter = iter(self._meta.concrete_fields)
            # The ordering of the zip calls matter - zip throws StopIteration
            # when an iter throws it. So if the first iter throws it, the second
            # is *not* consumed. We rely on this, so don't change the order
            # without changing the logic.
            for val, field in zip(args, fields_iter):
                setattr(self, field.attname, val)
        else:
            # Slower, kwargs-ready version.
            fields_iter = iter(self._meta.fields)
            for val, field in zip(args, fields_iter):
                setattr(self, field.attname, val)
                kwargs.pop(field.name, None)
                # Maintain compatibility with existing calls.
                if isinstance(field.remote_field, ManyToOneRel):
                    kwargs.pop(field.attname, None)
        ...
```

继续查看model的`save`函数，也没有发现转换字段类型的代码，
故得出结论：djano在创建model的时候，并不会转换字段类型，二是会保存原有的字段类型。

**解决方案**

如果你想在创建完model对象之后，还想继续使用model的字段，那么有两种方案处理这个问题：

* 方案1：

    在创建model对象的时候，直接使用字段对应的类型赋值。

* 方案2：

    在创建完对象后，调用model的`refresh_from_db`函数：

    ```python
    >>> m = DemoModel.objects.create(**{'str_field': 1})
    >>> type(m.str_field)
    <class 'int'>
    >>> m.str_field
    1
    >>> m.refresh_from_db()
    >>> type(m.str_field)
    <class 'str'>
    >>> m.str_field
    '1'
    ```

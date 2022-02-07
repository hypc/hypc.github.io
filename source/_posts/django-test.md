---
title: Django单元测试的几点建议
date: 2020-10-25 14:44:16
tags: [django, python, testing]
---

几天前有朋友问到怎么对Django项目做单元测试，于是，我将以前随手记下的笔记整理了一下。

* 测试用例继承`django.test.TestCase`。
* 如果在执行测试用例之前需要初始化全局数据，重写类函数`setUpTestData()`。
* 在测试用例中如果需要批量创建数据，请使用循环一个一个`create()`，不要使用`bulk_create()`，因为`bulk_create()`不会自动`save()`。
* 在测试过程中，灵活使用`mock`，推荐使用`unittest.mock.patch`，详细内容参见：[patchers](https://docs.python.org/3/library/unittest.mock.html#the-patchers)。
* 使用断言`assert`校验执行结果，不要使用`if..else..`判断，`django.test.TestCase`中已经内置一些`assert`，大家可以自由使用。
* 如果被测试的代码中使用了缓存，可以使用`django.core.cache.backends.dummy.DummyCache`或`django.core.cache.backends.locmem.LocMemCache`来防止缓存被污染。
* 做接口测试时，使用`self.client`发起请求。
* 测试需要登录时，使用`self.client.force_login(user)`进行登录。
* 如果测试需要携带请求头时，可以修改`self.client.defaults`的值，它是一个`dict`，直接`update`就行。
* 如果一个`app`下的测试用例代码非常多的话，建议将`tests.py`文件改成`tests/`文件夹。

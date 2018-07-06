---
title: Django测试需要登录的接口
date: 2018-07-06 09:55:06
tags: [django, python]
---

有时候需要测试一些需要登录的接口，或者测试一些需要特定权限才能调用的接口，而这些都需要登录，
在写测试用例的时候可以继承`django.test.TestCase`，
然后调用`self.client.login(self, **credentials)`或`self.client.force_login(self, user, backend=None)`来进行登录。

---
title: Django静态文件配置
date: 2019-04-24 11:15:07
tags: [django, python]
---

1. 首先，需要保证`INSTALLED_APPS`中有`django.contrib.staticfiles`模块：

    ![](/images/django-static-files-1.png)

2. 在`settings.py`中配置`STATIC_ROOT`：

    ![](/images/django-static-files-2.png)

    **注意**，这一步执行完之后可以执行`python manage.py collectstatic`将所有App的静态资源都收集到`STATIC_ROOT`目录下。

3. 在`urls.py`中添加静态文件路由配置：

    ![](/images/django-static-files-3.png)

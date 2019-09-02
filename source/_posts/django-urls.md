---
title: Django路由系统
date: 2019-08-22 13:19:20
tags: [django, python]
---

[Django][]的路由是怎么回事在这里不再赘述，这里主要说一下[`include()`][include]的用法，以及装配路由的一点技巧。

[Django]: https://www.djangoproject.com/ "The web framework for perfectionists with deadlines."
[include]: https://docs.djangoproject.com/en/2.2/ref/urls/#django.conf.urls.include

## include()

[`include`][include]主要有下面两种用法：

* `include(module, namespace=None)`
* `include(pattern_list)`

<!--more-->

### 用法一：`include(module, namespace=None)`

这种用法中，参数`module`类型为`str|object`，它的值是一个模块，或者是一个模块路径，可以正常定位这个模块，
`include`函数会自动加载`module`中的`urlpatterns`属性。
`urlpatterns`是一个列表，类型为`list[RegexURLResolver|RegexURLPattern]`。

例如：

```python
urlpatterns = [
    # ... snip ...
    url(r'^community/', include('django_website.aggregator.urls')),
    url(r'^contact/', include('django_website.contact.urls')),
    # ... snip ...
    url(r'^author-polls/', include('polls.urls', namespace='author-polls')),
    url(r'^publisher-polls/', include('polls.urls', namespace='publisher-polls')),
    # ... snip ...
]
```

### 用法二：`include(pattern_list)`

参数`pattern_list`是一个列表，类型为`list[RegexURLResolver|RegexURLPattern]`。

例如：

```python
polls_patterns = [
    url(r'^$', views.IndexView.as_view(), name='index'),
    url(r'^(?P<pk>\d+)/$', views.DetailView.as_view(), name='detail'),
]

urlpatterns = [
    url(r'^polls/', include(polls_patterns)),
]
```

## 自动装配路由

```python
# ##################################
# #### <django_project>/urls.py ####
# ##################################
from importlib import import_module

from django.conf import settings
from django.conf.urls import url, include

urlpatterns = []    # settings.INSTALLED_APPS之外的urls，可直接在这写

_c_urlpatterns, _p_urlpatterns = [], []   # 具有相同url前缀，但又分属不同模块的pattern_list
for app in settings.INSTALLED_APPS:
    if app.startswith('django.contrib'):    # 忽略django内置模块
        continue
    try:
        urlconf_module = import_module(app + '.urls')
        urlpatterns += getattr(urlconf_module, 'urlpatterns', [])
        _c_urlpatterns += getattr(urlconf_module, 'c_urlpatterns', [])
        _p_urlpatterns += getattr(urlconf_module, 'p_urlpatterns', [])
    except ImportError:
        pass

urlpatterns += [
    url(r'^cs/(?P<c_id>\w{1,32})', include(_c_urlpatterns)),
    url(r'^ps/(?P<p_id>\w{1,32})', include(_p_urlpatterns)),
]
```

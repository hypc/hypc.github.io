---
title: 关于Django数据库连接
date: 2020-10-05 19:09:17
tags: [django, python]
---

Django程序在接收到请求之后，会在第一次访问数据库时创建连接，到请求结束时关闭连接。

## 源码分析

```python
# django/core/handlers/wsgi.py
class WSGIHandler(base.BaseHandler):
    request_class = WSGIRequest

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.load_middleware()

    def __call__(self, environ, start_response):
        set_script_prefix(get_script_name(environ))
        # 请求开始，触发request_started信号
        signals.request_started.send(sender=self.__class__, environ=environ)
        request = self.request_class(environ)
        response = self.get_response(request)

        response._handler_class = self.__class__

        status = '%d %s' % (response.status_code, response.reason_phrase)
        response_headers = [
            *response.items(),
            *(('Set-Cookie', c.output(header='')) for c in response.cookies.values()),
        ]
        start_response(status, response_headers)
        if getattr(response, 'file_to_stream', None) is not None and environ.get('wsgi.file_wrapper'):
            # If `wsgi.file_wrapper` is used the WSGI server does not call
            # .close on the response, but on the file wrapper. Patch it to use
            # response.close instead which takes care of closing all files.
            response.file_to_stream.close = response.close
            response = environ['wsgi.file_wrapper'](response.file_to_stream, response.block_size)
        return response
```

<!--more-->

```python
# django/http/response.py
class HttpResponseBase:
    def close(self):
        for closer in self._resource_closers:
            try:
                closer()
            except Exception:
                pass
        # Free resources that were still referenced.
        self._resource_closers.clear()
        self.closed = True
        # 请求结束，触发request_finished信号
        signals.request_finished.send(sender=self._handler_class)
```

通过上面两段代码可以看出，Django会在请求开始触发`request_started`信号，在请求结束触发`request_finished`信号。这两个信号的作用是什么呢？

```python
# django/db/__init__.py
# Register an event to reset saved queries when a Django request is started.
def reset_queries(**kwargs):
    for conn in connections.all():
        conn.queries_log.clear()

signals.request_started.connect(reset_queries)

# Register an event to reset transaction state and close connections past
# their lifetime.
def close_old_connections(**kwargs):
    for conn in connections.all():
        conn.close_if_unusable_or_obsolete()

signals.request_started.connect(close_old_connections)
signals.request_finished.connect(close_old_connections)
```

不难看出，`request_started`、`request_finished`这两个信号都是遍历并关闭`connections`对象中的连接。
`connections`是一个`ConnectionHandler`对象，它用于管理数据库连接：

```python
class ConnectionHandler:
    def __init__(self, databases=None):
        """
        databases is an optional dictionary of database definitions (structured
        like settings.DATABASES).
        """
        self._databases = databases
        # Connections needs to still be an actual thread local, as it's truly
        # thread-critical. Database backends should use @async_unsafe to protect
        # their code from async contexts, but this will give those contexts
        # separate connections in case it's needed as well. There's no cleanup
        # after async contexts, though, so we don't allow that if we can help it.
        self._connections = Local(thread_critical=True)

    def __getitem__(self, alias):
        if hasattr(self._connections, alias):
            return getattr(self._connections, alias)

        self.ensure_defaults(alias)
        self.prepare_test_settings(alias)
        db = self.databases[alias]
        # 这里非常关键，它会先load_backend，然后创建连接，之后再将连接放入thread local中进行管理
        backend = load_backend(db['ENGINE'])
        conn = backend.DatabaseWrapper(db, alias)
        setattr(self._connections, alias, conn)
        return conn
```

将上面代码串联起来，便可以得出结论：接收到请求之后，在第一次访问数据库时创建连接，在请求结束时关闭连接。

## 与Celery结合

Celery处于多进程模式下，会出现一个致命问题，子进程fork出来后，它会与父进程共用相同的数据库连接，
这样极有可能导致连接不能被正常关闭。

可以通过手动关闭连接来解决这个问题：

```python
from celery.signals import task_prerun, task_postrun, worker_process_init
from django.db import close_old_connections

worker_process_init.connect(close_old_connections)
task_prerun.connect(close_old_connections)
task_postrun.connect(close_old_connections)
```

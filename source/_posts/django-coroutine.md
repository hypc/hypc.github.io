---
title: Django协程化
date: 2021-09-29 21:39:30
tags: [django]
---

## requirements

```requirements
django
gunicorn
gevent  # use this if you use gevent workers
# eventlet  # use this if you use eventlet workers
psycopg2-binary
psycogreen
django-db-geventpool
celery
```

<!--more-->

## gunicorn 服务

配置 `gunicorn` 配置文件 `gunicorn.conf.py`：

```python
worker_class = 'gevent'  # use this if you use gevent workers
# worker_class = 'eventlet'  # use this if you use eventlet workers
```

## psycopg2

配置 `gunicorn` 配置文件 `gunicorn.conf.py`：

```python
def post_fork(server, worker):
    from psycogreen.gevent import patch_psycopg  # use this if you use gevent workers
    # from psycogreen.eventlet import patch_psycopg  # use this if you use eventlet workers
    patch_psycopg()
```

配置 `settings.py` 文件：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django_db_geventpool.backends.postgresql_psycopg2',
        # 'ENGINE': 'django_db_geventpool.backends.postgis',  # use this if you use postgis
        # ...
    }
}
```

## celery

配置 `celery.py` 文件：

```python
from celery.signals import worker_ready


@worker_ready.connect
def _worker_ready():
    from psycogreen.gevent import patch_psycopg  # use this if you use gevent workers
    # from psycogreen.eventlet import patch_psycopg  # use this if you use eventlet workers
    patch_psycopg()
```

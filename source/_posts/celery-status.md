---
title: 查看Celery任务状态
date: 2020-08-04 19:10:47
tags: [python, celery]
---

[Celery][]是python中常用的一个异步任务队列，使用它可以简化搭建任务队列的工作。

[Celery]: https://docs.celeryproject.org/

## 使用命令查看

### 查看worker状态

**查看worker的状态：**

```bash
$ celery -A proj status
celery@e12d18b11bf8: OK
celery@4e8eb97af6a8: OK
celery@0d9dcbcd7862: OK
celery@e858cc9a321f: OK
celery@7254993a84be: OK
...
```


**查看worker的详细信息：**

```bash
$ celery -A proj inspect stats
-> celery@0d9dcbcd7862: OK
    {
        "broker": {...},
        "clock": "649",
        "pid": 1,
        "pool": {...},
        "prefetch_count": 16,
        "rusage": {...},
        "total": {}
    }
-> celery@4e8eb97af6a8: OK
    {
        "broker": {...},
        "clock": "649",
        "pid": 1,
        "pool": {...},
        "prefetch_count": 16,
        "rusage": {...},
        "total": {}
    }
...
```

<!--more-->

### 查看当前正在执行的task

```bash
$ celery -A proj inspect active
-> celery@ec8e264c5c0e: OK
    - empty -
-> celery@16c1f1a6d8ca: OK
    * {'id': 'b2626431-4420-4dde-a596-be7b63942ac4', 'name': 'examples.tasks.low_task', 'args': [], 'kwargs': {}, 'type': 'examples.tasks.low_task', 'hostname': 'celery@16c1f1a6d8ca', 'time_start': 1596941119.8071737, 'acknowledged': True, 'delivery_info': {'exchange': '', 'routing_key': 'low', 'priority': None, 'redelivered': False}, 'worker_pid': 7}
...
```

### 查看当前队列信息

```bash
$ celery -A proj inspect active_queues
-> celery@d6dd3883d8a8: OK
    * {'name': 'default', 'exchange': {'name': 'default', 'type': 'direct', 'arguments': None, 'durable': True, 'passive': False, 'auto_delete': False, 'delivery_mode': None, 'no_declare': False}, 'routing_key': 'default', 'queue_arguments': None, 'binding_arguments': None, 'consumer_arguments': None, 'durable': True, 'exclusive': False, 'auto_delete': False, 'no_ack': False, 'alias': None, 'bindings': [], 'no_declare': None, 'expires': None, 'message_ttl': None, 'max_length': None, 'max_length_bytes': None, 'max_priority': None}
-> celery@e12d18b11bf8: OK
    * {'name': 'default', 'exchange': {'name': 'default', 'type': 'direct', 'arguments': None, 'durable': True, 'passive': False, 'auto_delete': False, 'delivery_mode': None, 'no_declare': False}, 'routing_key': 'default', 'queue_arguments': None, 'binding_arguments': None, 'consumer_arguments': None, 'durable': True, 'exclusive': False, 'auto_delete': False, 'no_ack': False, 'alias': None, 'bindings': [], 'no_declare': None, 'expires': None, 'message_ttl': None, 'max_length': None, 'max_length_bytes': None, 'max_priority': None}
-> celery@e05921129af4: OK
    * {'name': 'high', 'exchange': {'name': 'high', 'type': 'direct', 'arguments': None, 'durable': True, 'passive': False, 'auto_delete': False, 'delivery_mode': None, 'no_declare': False}, 'routing_key': 'high', 'queue_arguments': None, 'binding_arguments': None, 'consumer_arguments': None, 'durable': True, 'exclusive': False, 'auto_delete': False, 'no_ack': False, 'alias': None, 'bindings': [], 'no_declare': None, 'expires': None, 'message_ttl': None, 'max_length': None, 'max_length_bytes': None, 'max_priority': None}
...
```

### 其他

celery命令还提供一些其他功能，就不一一列举了，大家可以通过`--help`查看：

```bash
$ celery -A proj inspect --help
...
[Commands]
|    active
|        List of tasks currently being executed.
|    active_queues
|        List the task queues a worker is currently consuming from.
|    clock
|        Get current logical clock value.
|    conf [include_defaults=False]
|        List configuration.
|    memdump [n_samples=10]
|        Dump statistics of previous memsample requests.
|    memsample
|        Sample current RSS memory usage.
|    objgraph [object_type=Request] [num=200 [max_depth=10]]
|        Create graph of uncollected objects (memory-leak debugging).
|    ping
|        Ping worker(s).
|    query_task [id1 [id2 [... [idN]]]]
|        Query for task information by id.
|    registered [attr1 [attr2 [... [attrN]]]]
|        List of registered tasks.
|    report
|        Information about Celery installation for bug reports.
|    reserved
|        List of currently reserved tasks, not including scheduled/active.
|    revoked
|        List of revoked task-ids.
|    scheduled
|        List of currently scheduled ETA/countdown tasks.
|    stats
|        Request worker statistics/information.
```

## 使用Flower

[Flower][]是一个用来监视和管理[Celery][]集群的web工具。

[Flower]: https://flower.readthedocs.io/en/latest/

安装：

```bash
$ pip install flower
```

使用：

```bash
$ flower -A proj --port=5555
$ celery flower -A proj --address=127.0.0.1 --port=5555
```

效果如下：

![](/images/celery-status-1.png)

![](/images/celery-status-2.png)

## 在代码中查看

有些时候，需要将celery的监控嵌入到自己的系统中，或者想要了解celery更多的细节，
那你就需要通过代码来查询celery的状态信息。

```python
from proj.celery import app

# @see: celery.app.control.Inspect
inspect = app.control.inspect()
# 查看worker状态
inspect.stats()
# 查看当前运行的任务
inspect.active()
# 查看当前接收但未执行的任务
inspect.reserved()
# 查看queues
inspect.active_queues()
```

**注意：**代码中的`app`需要使用`Celery()`实例化之后的对象，而不能使用celery提供的`celery.current_app`对象。
否则，代码将会处于堵塞状态，导致拿不到想要的数据。

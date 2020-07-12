---
title: 使用django-celery-beat处理大量任务时异常
date: 2020-07-04 19:06:32
tags: [django, celery, python]
---

今天有同事问我一个问题，在使用[django-celery-beat][]库时，发现有大量定时任务没有执行。
我一听到这就有点疑惑：有大量定时任务？

[django-celery-beat]: https://django-celery-beat.readthedocs.io/

在了解了详细情况后发现，这个业务本身是应该采用其他方式处理的，如：

1. 创建一个每N分钟执行的定时任务，然后在这个任务中对业务进行批量处理；
2. 为每一条业务数据生成一个异步任务，而非定时任务。

虽说业务可以绕过去，但问题本身并没有解决：`有大量定时任务没有执行`。

## 问题原因分析

### 创建大量定时任务

```python
# 总定时任务数：24 * 60 * 100 == 144000
for hour, minute in product(range(24), range(60)):
    tasks = []
    crontab, _ = CrontabSchedule.objects.get_or_create(minute=minute, hour=hour)
    for index in range(100):
        tasks.append(PeriodicTask(
            crontab=crontab,
            name=f'beat_{hour}_{minute}_{index}',
            task='examples.tasks.task_schedule'
        ))
    PeriodicTask.objects.bulk_create(tasks)
```

<!--more-->

### 启动worker及beat

```bash
# 启动worker
celery -A celery_demo worker -l info -O fair
# 启动beat
celery -A celery_demo beat -l info
```

通过观察发现，beat启动加载定时任务大概需要3.5分钟，发送定时任务大概需要1.5秒。

### 结论

由上面观察得出，当存在大量定时任务时，beat加载任务及发送任务都变得非常缓慢。
如果动态创建定时任务，将会触发`PeriodicTasks.changed`事件，这会导致beat重新加载定时任务。

本文开头的那个问题，便是由于动态实时的创建定时任务，beat一直处于加载定时任务的过程中，从而导致定时任务没有执行。

## 解决问题

### 定时任务加载慢的问题

通过查看源码，发现下面代码：

```python
class DatabaseScheduler(Scheduler):
    def all_as_schedule(self):
        debug('DatabaseScheduler: Fetching database schedule')
        s = {}
        for model in self.Model.objects.enabled():
            try:
                s[model.name] = self.Entry(model, app=self.app)
            except ValueError:
                pass
        return s
```

注意看第5行代码，这里只是将`PeriodicTask`对象查找出来了，
并没有将关联对象`interval`、`crontab`、`solar`、`clocked`同步查找出来，而在后续使用的过程中，它将会再次触发数据库查询。
最终产生了大量的数据库查询，导致加载定时任务变慢。

#### 优化方案一：采用连表查询的方式

```python
def all_as_schedule(self):
    s = {}
    for model in self.Model.objects.select_related(
            'interval', 'crontab', 'solar', 'clocked').filter(enabled=True):
        try:
            s[model.name] = self.Entry(model, app=self.app)
        except ValueError:
            pass
    return s
```

使用连表查询的方式，关联对象的数量与主对象的数量一样多，而实际上关联对象并没有多少（存在大量复用的情况）。
这样浪费大量时间在传输数据上，而且也会创建大量重复对象。

故，不建议采用这种方式，建议采用方案二。

#### 优惠方案二：分别查询然后组合数据

```python
def all_as_schedule(self):
    s = {}
    intervals_dict = {_.id: _ for _ in IntervalSchedule.objects.all()}
    crontabs_dict = {_.id: _ for _ in CrontabSchedule.objects.all()}
    solars_dict = {_.id: _ for _ in SolarSchedule.objects.all()}
    clockeds_dict = {_.id: _ for _ in ClockedSchedule.objects.all()}
    for model in self.Model.objects.enabled():
        try:
            model.interval = intervals_dict.get(model.interval_id, None)
            model.crontab = crontabs_dict.get(model.crontab_id, None)
            model.solar = solars_dict.get(model.solar_id, None)
            model.clocked = clockeds_dict.get(model.clocked_id, None)
            s[model.name] = self.Entry(model, app=self.app)
        except ValueError:
            pass
    return s
```

### 发送定时任务慢的问题

查看源码，发现下面代码：

```python
class DatabaseScheduler(Scheduler):
    def schedules_equal(self, *args, **kwargs):
        if self._heap_invalidated:
            self._heap_invalidated = False
            return False
        return super(DatabaseScheduler, self).schedules_equal(*args, **kwargs)

class Scheduler(object):
    def schedules_equal(self, old_schedules, new_schedules):
        if old_schedules is new_schedules is None:
            return True
        if old_schedules is None or new_schedules is None:
            return False
        if set(old_schedules.keys()) != set(new_schedules.keys()):
            return False
        for name, old_entry in old_schedules.items():
            new_entry = new_schedules.get(name)
            if not new_entry:
                return False
            if new_entry != old_entry:
                return False
        return True
```

通过断点调试，发现时间主要浪费在`for name, old_entry in old_schedules.items():`这个循环体内。

另外还发现，`old_schedules`、`new_schedules`都是`dict`，且`old_schedules is not new_schedules`，`old_schedules == new_schedules`。

故这里可以直接优化成：

```python
def schedules_equal(self, *args, **kwargs):
    if self._heap_invalidated:
        self._heap_invalidated = False
        return False
    return old_schedules == new_schedules
```

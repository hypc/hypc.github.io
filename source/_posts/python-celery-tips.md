---
title: celery的建议
date: 2018-07-25 14:29:28
tags: [python, celery]
---

## 按业务场景划分队列

一定一定要更具业务特点来划分出不同的队列，不能将所有的任务都让同一队列消费。
在实际使用异步任务的场景中，一定会有优先级不高的任务（如统计类）和优先级特别高的任务（如微信红包）。
若不划分队列，那么当统计类的异步任务很多很多的时候，
发送红包给微信用户的异步任务就要等待一段时间才能被执行到（任务多可能会等待十几分钟甚至一两个小时），这对用户体验来说是非常不友好的。
此时就可以将队列划分为`low`，`middle`，`high`这几种队列，对于统计类的这些任务可以扔去`low`的队列中，
而对于发送红包这种重要的任务就扔去`high`队列中，确保尽可能快的被执行到。具体划分，需要更加业务场景来划分。

通过`-Q`参数来指定队列名：

```bash
celery -A proj worker -l info -Q low,middle,high
```

```python
# 方式1
@app.task(bind=True, queue='middle', name='send_wx_text')
def send_wx_text(self, target_origin_id, to_user, txt):
    pass

# 方式2
send_wx_text.apply_async((target_origin_id, to_user, txt), queue='middle')
```

<!--more-->

## 对任务返回结果的处理

大部分异步任务都是不需要关心处理结果的，此时不需要将处理结果返回。
由于返回处理结果，需要存储，这意味着有IO操作，会降低性能，若将结果存储在redis或者RabbitMQ中，
随着任务数的增加，消耗的内存也会增加（曾经因为这个原因爆过内存.....）。
若非要存储结果，也要设置好结果的保存时间，以免内存泄漏。

```python
# 设置结果的保存时间
CELERY_TASK_RESULT_EXPIRES = 10*60
# 设置默认不存结果
CELERY_IGNORE_RESULT = True

# 任务单独指定, 会覆盖全局配置
@app.task(bind=True, ignore_result=True)
def send_wx_text(self, target_origin_id, to_user, txt):
    pass
```

## 对失败进行重试处理

* 明确哪些失败或错误是需要重试的，不要无条件的重试
* 设定最大重试次数(`max_retries`)，避免无限重试
* 重试的延时最好是更具重试次数进行增量延时，给更多的时间让等待服务恢复正常，以提高成功率

```python
def backoff(attempts):
    """
    1, 2, 4, 8, 16, 32, ...
    """
    return 2 ** attempts

# 设置max_retries， 限定重试次数
@app.task(bind=True, max_retries=3)
def send_wx_text(self, target_origin_id, to_user, txt):
    try:
        r, err = wx_api.send_text(to_user, txt)
        if err and int(err.code) in (45047):
            # 更具重试次数增加重试的延时的时间
            raise self.retry(args=[target_origin_id, to_user, txt],
                             countdown=backoff(self.request.tretries))
    except BaseException as e:
        raise e
```

## 尽快失败而不阻塞

对于一些爬虫类的异步任务，我们常常是使用代理去爬，一般来说代理服务商提供的代理能用的概率比较低，
容易导致timeout，对于这些可能会失败的任务应该尽快让它失败而不是在等待它失败，
避免做无畏的等待，进而提高性能。预估一下任务的正常处理时间，根据这个时间来规定一个任务的最大执行时间。

```python
@app.task(bind=True, max_retries=3, soft_time_limit=5)
def send_wx_text(self, target_origin_id, to_user, txt):
    pass
```

## 对任务进行通用处理

任务失败或者重试，一般来说是需要进行记录，若每一个任务都写一遍相关的代码，这非常麻烦。
此时，通过执行任务的base参数对任务进行统一处理。

```python
from myproject.tasks import app

# 具体的可以看看Task类中的方法
class BaseTask(app.Task):
    abstract = True

    def on_retry(self, exc, task_id, args, kwargs, einfo):
        sentrycli.captureException(exc)
        super(BaseTask, self).on_retry(exc, task_id, args, kwargs, einfo)

    def on_failure(self, exc, task_id, args, kwargs, einfo):
        sentrycli.captureException(exc)
        super(BaseTask, self).on_failure(exc, task_id, args, kwargs, einfo)


@app.task(bind=True, max_retries=3, soft_time_limit=5, ignore_result=True, base=BaseTask)
def send_wx_text(self, target_origin_id, to_user, txt):
    pass
```

## 使用RabbitMQ作为Broker

* 更稳定
* 消耗内存比redis要小，用redis做broker当任务量很多时，内存极剧上升，估计有内存泄漏问题
* rabbitmq-plugins这监控工具非常好用(用flower作为监控工具，
    在测试服务器部署跑，内存会随着时间的推移而不断增加，估计有内存泄漏问题)

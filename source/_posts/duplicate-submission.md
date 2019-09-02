---
title: 防止表单重复提交
date: 2019-08-22 10:07:45
tags: [design]
---

在一些特殊场景下，可能会导致表单重复提交，如：

* 网络延时，用户多次点击提交
* 刷新页面导致重复提交表单
* 后退之后再次提交表单
* ...

有些幂等操作允许重复提交表单，但大多数操作都是非幂等的，重复提交将会导致系统异常甚至系统崩溃。
所以，我们需要尽量避免表单重复提交。

这里有两种思路：

* 前端拦截，这可以拦截绝大部分用户误操作
* 后端处理

<!--more-->

## 前端处理

表单中添加隐藏参数`noncestr`（是一个随机字符串，用来标识表单），提交表单时，`noncestr`一起提交，
然后锁定表单，将表单状态设置为不可提交（或一段时间内不可提交），
如果对表单再次编辑，修改`noncestr`值，解除表单锁定状态。

## 后端处理

1. 先从cache中查询`noncestr`对应操作，
2. 如果查询到了对应的操作，判断操作的处理状态
   1. 已经完成，直接将操作对应的结果返回
   2. 未完成，返回202响应，告知客户端已接受该请求，并正在处理中
3. 如果未查询到对应的操作
   1. 查询数据库，看是否有对应操作的记录
   2. 如果有对应的操作记录，将操作结果存放到缓存中，然后返回处理结果
   3. 如果没有，进行正常的业务处理，将操作结果存放到缓存中，然后返回处理结果

Python代码如下：

```python
cache = get_cache()   # 缓存
timeout = 30    # 缓存超时时间

cache_key = '$(session_id)_$(noncestr)'
value = cache.get(cache_key)
if value is not None and value['status'] == 'doing':
    return Response(202)
elif value is not None and value['status'] == 'done':
    return value.response
else:
    cache.set(cache_key, {'status': 'doing'}, timeout)
    response = do_something()   # 业务处理
    cache.set(cache_key, {'status': 'done', 'response': response}, timeout)
    return response

def do_something():
    db = get_db()   # 数据库
    data = db.get(noncestr) # 查询时，可以附带其他条件防止误查
    if data is not None:
        return Response(data)
    else:
        data = ...  # 正常业务处理
        return Response(data)
```

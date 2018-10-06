---
title: Python线程之Semaphore同步
date: 2018-09-30 13:16:19
tags: [python]
---

Semaphore，是一种带计数的线程同步机制，当调用release时，增加计数，当acquire时，减少计数，当计数为0时，自动阻塞，等待release被调用。

在Python中存在两种Semaphore，一种就是纯粹的Semaphore，另一种是BoundedSemaphore。

* `Semaphore`: 在调用`release()`函数时，不会检查，增加的计数是否超过上限（没有上限，会一直上升）
* `BoundedSemaphore`: 在调用`release()`函数时，会检查，增加的计数是否超过上限，这样就保证了使用的计数

**示例**：

```python
maxconnections = 5
# ...
pool_sema = BoundedSemaphore(value=maxconnections)

# ...

with pool_sema:
    # 这里需要注意，使用with语句块，会自动帮你执行acquire()和release()
    conn = connectdb()
    try:
        # ... use connection ...
    finally:
        conn.close()
```

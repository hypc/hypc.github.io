---
title: Python ThreadLocal
date: 2019-08-28 16:43:01
tags: [python]
---

我们知道多线程环境下，每一个线程均可以使用所属进程的全局变量。
如果一个线程对全局变量进行了修改，将会影响到其他所有的线程。
为了避免多个线程同时对变量进行修改，引入了线程同步机制，通过互斥锁，
条件变量或者读写锁来控制对全局变量的访问。

只用全局变量并不能满足多线程环境的需求，很多时候线程还需要拥有自己的私有数据，
这些数据对于其他线程来说不可见。
因此线程中也可以使用局部变量，局部变量只有线程自身可以访问，同一个进程下的其他线程不可访问。

下图给出了线程中这几种变量的存在情况：

![](/images/python-thread-local-1.png)

<!--more-->

## ThreadLocal

有时候使用局部变量不太方便，例如，在实际生产环境中，我们需要调用大量函数，
每个函数都需要很多局部变量，这时候用传递参数的方法会非常繁琐。

为了解决这个问题，一个直观的的方法就是建立一个全局字典，保存进程ID到该进程局部变量的映射关系，
运行中的线程可以根据自己的ID来获取本身拥有的数据。这样，就可以避免在函数调用中传递参数。例如：

```python
import threading
import time

global_data = {}

def func():
    key = id(threading.current_thread())
    if not global_data.get(key):
        global_data[key] = 0
    for _ in range(10000):
        global_data[key] += 1
    print(threading.current_thread().getName(), global_data[key])

if __name__ == '__main__':
    for _ in range(20):
        thread = threading.Thread(target=func)
        thread.start()
    time.sleep(10)
```

Python还提供了ThreadLocal变量，它本身是一个全局变量，
但是每个线程却可以利用它来保存属于自己的私有数据，这些私有数据对其他线程也是不可见的。

ThreadLocal示例：

```python
import threading
import time

global_data = threading.local()

def func():
    if not hasattr(global_data, 'num'):
        global_data.num = 0
    for _ in range(10000):
        global_data.num += 1
    print(threading.current_thread().getName(), global_data.num)

if __name__ == '__main__':
    for _ in range(20):
        thread = threading.Thread(target=func)
        thread.start()
    time.sleep(10)
```

上面示例中每个线程都可以通过`global_data.num`获得自己独有的数据，
并且每个线程读取到的`global_data`都不同，真正做到线程之间的隔离。

## ThreadLocal的实现原理

其实ThreadLocal的原理特别简单，就是创建一个全局字典，然后将线程的标识符作为key，
相应线程的数据作为value。

感兴趣的童鞋可以阅读`threading.local`源码，里面一些编辑技巧非常值得学习。

## ThreadLocal的局限

Python自带的ThreadLocal只能基于线程使用，而以下两种情况并不适合使用ThreadLocal：

1. 协程：一些阻塞的操作，我们经常会使用到协程（如gevent）进行处理；
2. 线程池：在WSGI中，并不能保证每次都产生一个新的线程来处理请求，
   那么上一个请求遗留下来的数据将会污染当前请求。

要解决这两个问题，需要设计一个Local类，它能做到线程以及协程之间数据的隔离，
并且支持清理某个线程或协程下的数据。

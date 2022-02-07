---
title: contextvars
date: 2021-12-23 15:17:53
tags: [python]
---

[contextvars][] 用于管理、存储和访问上下文相关的状态。

[contextvars]: https://docs.python.org/zh-cn/3/library/contextvars.html

在了解 contextvars 之前，我们先了解一下 ThreadLocal。

## ThreadLocal

顾名思义，ThreadLocal 就是本地线程变量，它可用来管理当前线程中的变量。
主要有以下几种使用场景：

1. 跨层传输数据的时候，可以简化参数传递，打破层与层之间的屏障
2. 隔离线程之间的数据

用法大致如下：

```python
import threading
import time

global_data = threading.local()

def func():
    if not hasattr(global_data, 'num'):
        global_data.num = 0
    for _ in range(100):
        global_data.num += 1
    print(threading.current_thread().getName(), global_data.num)

if __name__ == '__main__':
    for _ in range(20):
        thread = threading.Thread(target=func)
        thread.start()
    time.sleep(10)
```

ThreadLocal 不适合使用的场景：

1.  协程：一些阻塞的操作，我们经常会使用到协程（如 gevent）进行处理；
2.  线程池：在 WSGI 中，并不能保证每次都产生一个新的线程来处理请求， 那么上一个请求遗留下来的数据将会污染当前请求。

<!--more-->

## contextvars

ThreadLocal 虽然在线程中隔离效果特别好，但是在协程中效果并不理想，
于是 contextvars 便出现了，它用于管理、存储和访问上下文相关的状态。

contextvars 在 asyncio 中有原生的支持并且无需任何额外配置即可被使用。
例如，以下是一个简单的回显服务器，它使用 contextvars 来将客户端的地址传递到请求处理的协程中：

```python
import asyncio
import contextvars

client_addr_var = contextvars.ContextVar('client_addr')

def render_goodbye():
    # The address of the currently handled client can be accessed
    # without passing it explicitly to this function.

    client_addr = client_addr_var.get()
    return f'Good bye, client @ {client_addr}\n'.encode()

async def handle_request(reader, writer):
    addr = writer.transport.get_extra_info('socket').getpeername()
    client_addr_var.set(addr)

    # In any code that we call is now possible to get
    # client's address by calling 'client_addr_var.get()'.

    while True:
        line = await reader.readline()
        print(line)
        if not line.strip():
            break
        writer.write(line)

    writer.write(render_goodbye())
    writer.close()

async def main():
    srv = await asyncio.start_server(
        handle_request, '127.0.0.1', 8081)

    async with srv:
        await srv.serve_forever()

asyncio.run(main())
```

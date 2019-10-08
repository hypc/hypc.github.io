---
title: Python实现控制台进度条
date: 2019-10-08 13:36:39
tags: [python]
---

要实现进度条功能，只需要注意以下两点就可以做到：

* 使用`\r`符号，从行首进行输出
* `print`时不要换行，即使用`end=''`

```python
from datetime import datetime
from time import sleep


class ProcessBar(object):
    fmt = '\r\033[32m{percent:.2%}\033[0m' \
          ' (\033[32m{cur}\033[0m of \033[31m{total}\033[0m)' \
          ' Elapsed time: \033[33m{elapsed}\033[0m' \
          ' Estimated finish time: \033[34m{estimated}\033[0m'

    def __init__(self, total, cur=0):
        self._begin_time = None
        self._total = total
        self._cur = cur

    def print(self, cur):
        if self._begin_time is None:
            self._begin_time = datetime.now()
        dt = datetime.now()
        du = dt - self._begin_time
        percent = cur / self._total
        print(self.fmt.format(**{
            'percent': percent,
            'cur': cur,
            'total': self._total,
            'elapsed': du,
            'estimated': self._begin_time + (du / percent) if cur else '--',
        }), end='')

    def begin(self):
        self._begin_time = datetime.now()
        self.print(self._cur)

    def end(self):
        self.print(self._total)
        print()


if __name__ == '__main__':
    p = ProcessBar(123)
    p.begin()
    for i in range(123):
        sleep(0.1)  # process ...
        p.print(i + 1)
    p.end()
```

示例代码是一个彩色进度条，关于彩色的控制可参考我的另一篇文章：{% post_link terminal-colorful Linux终端彩色输出 %}。

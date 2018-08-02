---
title: 扩展JSONEncoder
date: 2018-08-02 20:05:07
tags: [python]
---

```python
import json
import uuid


class MyClass(object):

    def __init__(self, name):
        self.uid = uuid.uuid1()
        self.name = name

    def info(self):
        return {
            'uid': self.uid.hex,
            'name': self.name,
        }


class MyJSONEncoder(json.JSONEncoder):

    def default(self, o):
        if isinstance(o, MyClass):
            # 返回一个可json化的数据，可以是任何类型的数据，只要它最终能转化为基础类型即可
            return o.info()
        # 最后调用父类的default函数，最终会调用基类的default函数
        return super(MyJSONEncoder, self).default(o)
```

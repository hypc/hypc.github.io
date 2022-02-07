---
title: Django：导出数据
date: 2021-10-13 14:59:39
tags: [django]
---

有4个关键点：

1. 对于导出数据中任意一行，尽量做到只做一次数据库查询，如果 `Django` 原生不支持，那就写 `SQL` 语句
2. 只查询导出的字段，不要查询不需要的字段
3. 分批获取数据，建议每次获取 10000 行（并非绝对，可以先用 SQL 语句在数据库查询，取一个较优的数值）
4. 使用 `StreamingHttpResponse` 做流式响应（文本文件可以边查边响应，二进制文件就在临时文件生成之后再响应）

```python
from django.db.models import Q
from django.http import StreamingHttpResponse

from sample.models import Bicycle


def export_csv(request):
    heads = ('bicycle_num', 'bicycle_type', 'body_num',
             'platform_qrcode', 'put_status', 'bicycle_qrcode', 'created_time',)

    def query_data():
        q = Q()
        limit, offset = 10000, 0
        while True:
            bicycles = Bicycle.objects.filter(q).values(*heads)[offset: offset + limit]
            if len(bicycles) == 0:
                break
            yield bicycles
            offset += limit

    def _to_csv():
        yield ','.join(heads) + '\n'
        for data in query_data():
            for row in data:
                yield ','.join([f'"{row[head]}"' for head in heads]) + '\n'

    return StreamingHttpResponse(_to_csv(), headers={
        'content_type': 'application/csv',
        'content-disposition': 'attachment; filename=export.csv',
    })
```

<!--more-->

## 优化

```python
import json

from django.http import StreamingHttpResponse


class Exporter(object):
    def __init__(self, data=(), *, heads=(), filename='export'):
        self.data = data
        self.heads = heads
        self.filename = filename

    def _to_csv(self):
        yield ','.join(self.heads) + '\n'
        for data in self.data:
            for row in data:
                yield ','.join([f'"{row[head]}"' for head in self.heads]) + '\n'

    def _to_xml(self):
        yield '<?xml version="1.0" encoding="utf-8"?>\n<data>\n'
        for data in self.data:
            for row in data:
                yield dict_to_xml(row) + '\n'
        yield '</data>'

    def _to_tsv(self):
        yield '\t'.join(self.heads) + '\n'
        for data in self.data:
            for row in data:
                yield '\t'.join([f'"{row[head]}"' for head in self.heads]) + '\n'

    def _to_json(self):
        yield '{ "data": ['
        is_first = True
        for data in self.data:
            for row in data:
                if is_first:
                    is_first = False
                    yield '  ' + json.dumps(row)
                else:
                    yield ',\n  ' + json.dumps(row)
        yield '\n]}'

    def _to_xlsx(self):
        raise Exception('')

    def export(self, fmt='csv'):
        return StreamingHttpResponse(getattr(self, f'_to_{fmt}')(), headers={
            'content_type': f'application/{fmt}',
            'content-disposition': f'attachment; filename={self.filename}.{fmt}',
        })
```

**Usage**

```python
class ExportView(View):
    heads = ('bicycle_num', 'bicycle_type', 'body_num',
             'platform_qrcode', 'put_status', 'bicycle_qrcode', 'created_time',)

    def get(self, request):
        form = ExportForm(request.GET)
        if not form.is_valid():
            fmt = 'csv'
        else:
            fmt = form.cleaned_data['fmt']

        q = Q()
        bicycles = Bicycle.objects.filter(q)

        def query():
            offset, limit = 0, 10000
            while True:
                data = bicycles[offset: offset + limit]
                if len(data) == 0:
                    break
                yield [row.detail_info() for row in data]
                offset += limit

        return Exporter(query(), heads=self.heads, filename='bicycles').export(fmt)
```

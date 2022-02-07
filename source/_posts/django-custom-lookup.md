---
title: Django：自定义Lookup
date: 2021-03-23 18:51:31
tags: [django]
---

[Django][]已经内置了许多[lookup][field-lookups]用来检索数据，这些lookups能满足绝大部分应用场景。
但有些时候我们需要自定义lookup用来满足特定的需求。

[Django]: https://www.djangoproject.com/
[field-lookups]: https://docs.djangoproject.com/en/3.1/ref/models/querysets/#field-lookups
[lookups]: https://docs.djangoproject.com/en/3.1/ref/models/lookups/

假设需要实现如下查询需求：

```sql
select * from test_tbl reverse(content) like '54321%';  -- postgresql
```

定义如下`Lookup`：

```python
from django.db.models import Field
from django.db.models.lookups import Lookup

@Field.register_lookup
class ReversePattern(Lookup):
    lookup_name = 'reverse'

    def as_sql(self, compiler, connection):
        lhs, _ = self.process_lhs(compiler, connection)
        rhs, rhs_params = self.process_rhs(compiler, connection)
        return 'reverse(%s) like %s' % (lhs, rhs), [''.join(reversed(str(rhs_params[0]))) + '%']
```

查询：

```python
Test.objects.filter(content__reverse='12345')
```

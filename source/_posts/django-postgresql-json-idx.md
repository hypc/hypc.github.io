---
title: Django：Postgresql JSON索引
date: 2021-03-16 11:21:00
tags: [django, python, postgresql]
---

使用`db_index`参数创建索引只会创建`btree`索引：

```python
class JsonIdx(models.Model):
    content = models.JSONField(db_index=True)
```

```sql
CREATE INDEX json_idx_jsonidx_content_d01e171b
    ON public.json_idx_jsonidx USING btree
    (content ASC NULLS LAST)
    TABLESPACE pg_default;
```

<!--more-->

创建`gin`索引：

```python
class JsonIdx(models.Model):
    content = models.JSONField()

    class Meta:
        indexes = [
            GinIndex(fields=['content'])
        ]
```

```sql
CREATE INDEX json_idx_js_content_61b1a2_gin
    ON public.json_idx_jsonidx USING gin
    (content)
    TABLESPACE pg_default;
```

创建`gin`索引，`opclass=jsonb_path_ops`：

```python
class JsonIdx(models.Model):
    content = models.JSONField()

    class Meta:
        indexes = [
            GinIndex(name='json_idx_gin_idx', fields=['content'], opclasses=['jsonb_path_ops'])
        ]
```

```sql
CREATE INDEX json_idx_gin_idx
    ON public.json_idx_jsonidx USING gin
    (content jsonb_path_ops)
    TABLESPACE pg_default;
```

结合文章{% post_link postgresql-json-idx Postgresql：JSON索引 %}，建议采用最后一种方式创建索引。

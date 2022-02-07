---
title: Django：Postgresql BTree索引
date: 2021-03-18 16:28:00
tags: [django, python, postgresql]
---

使用`db_index`参数创建`btree`索引，当字段类型是`varchar`或`text`时会额外创建`like index`，
具体参见：[DatabaseSchemaEditor._create_like_index_sql][]：

[DatabaseSchemaEditor._create_like_index_sql]: https://github.com/django/django/blob/6efc35b4fe3009666e56a60af0675d7d532bf4ff/django/db/backends/postgresql/schema.py#L69

```python
class BTreeIdx(models.Model):
    content = models.CharField(max_length=200, db_index=True)
```

```sql
CREATE INDEX btree_idx_btreeidx_content_511ce749
    ON public.btree_idx_btreeidx USING btree
    (content COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE pg_default;

CREATE INDEX btree_idx_btreeidx_content_511ce749_like
    ON public.btree_idx_btreeidx USING btree
    (content COLLATE pg_catalog."default" varchar_pattern_ops ASC NULLS LAST)
    TABLESPACE pg_default;
```

<!--more-->

创建`btree`索引：

```python
class BTreeIdx(models.Model):
    content = models.CharField(max_length=200)

    class Meta:
        indexes = [
            BTreeIndex(fields=['content'])
        ]
```

```sql
CREATE INDEX btree_idx_b_content_93bf39_btree
    ON public.btree_idx_btreeidx USING btree
    (content COLLATE pg_catalog."default" ASC NULLS LAST)
    TABLESPACE pg_default;
```

创建`btree`索引，`opclass=varchar_pattern_ops`：

```python
class BTreeIdx(models.Model):
    content = models.CharField(max_length=200)

    class Meta:
        indexes = [
            BTreeIndex(name='btree_idx_like_idx', fields=['content'], opclasses=['varchar_pattern_ops'])
        ]
```

```sql
CREATE INDEX btree_idx_like_idx
    ON public.btree_idx_btreeidx USING btree
    (content COLLATE pg_catalog."default" varchar_pattern_ops ASC NULLS LAST)
    TABLESPACE pg_default;
```

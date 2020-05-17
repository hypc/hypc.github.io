---
title: PostgreSQL导出表结构
date: 2020-05-10 22:04:33
tags: [postgresql]
---

这几天公司忙着做CMMI3认证，其中有一项是关于数据库设计的，需要以word文档的形式列举现有的表结构。
如果一个一个字段复制粘贴，那也太费时费力了，而且也非常容易遗漏。故需要有一种更简捷方便的方式导出表结构。

经查，`pg_dump`可以一次性将库中所有表结构导出来：

```bash
pg_dump -h host -p port -U username -s -t tablename dbname > struct.sql
```

* `-s`：只导表结构，不会导出数据
* `-t`：指定要导出的数据库表

这样导出的是sql语句格式的表结构，虽然比一个一个字段复制粘贴的方式好一些（文本编辑器的多光标操作），但也费时费力。

再查，发现PostgreSQL中有一个内置的表`information_schema.columns`，使用pgadmin执行下面sql语句：

<!--more-->

```sql
select table_name, column_name,
    case data_type
        when 'character varying' then data_type || '(' || character_maximum_length || ')'
        when 'numeric' then data_type || '(' || numeric_precision || ',' || numeric_scale || ')'
        else data_type
    end as data_type,
    case is_nullable
        when 'NO' then 'NOT NULL'
        else ''
    end as is_nullable
from information_schema.columns
where table_schema='public'
order by table_name, ordinal_position;
```

执行结果如下：

![](/images/postgresql-export-table-structure.png)

下载执行结果，是一个csv文件，然后直接用excel打开，稍做处理后可直接复制到word文件中。

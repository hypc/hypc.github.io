---
title: Postgresql：JSON索引
date: 2021-03-12 15:18:07
tags: [postgresql]
---

Name            | Indexable Operators           | Comments
----------------|-------------------------------|--------------------------------
jsonb_ops       | `?` `?&` `?\|` `@>` `@?` `@@`  | is default
jsonb_path_ops  | `@>` `@?` `@@`                | 支持少量的操作，但能提供更好的表现

```sql
-- drop table if exists test;
create table if not exists test (id int, js jsonb);

create index if not exists test_js_idx on test using gin (js jsonb_path_ops);

-- 插入1000w条数据花了近1个小时
insert into test (id, js)
select n, row_to_json(row(uuid_generate_v1(), uuid_generate_v1(), uuid_generate_v1(), uuid_generate_v1(), uuid_generate_v1()))
from generate_series(1, 10000000);

-- 查询时间在100ms～130ms之间
select * from test where js @> jsonb '{"f2": "5e36a48c-86f3-11eb-b9d7-0242ac120005"}';
```

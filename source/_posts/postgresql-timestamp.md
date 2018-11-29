---
title: PostgreSQL日期时间
date: 2018-11-29 09:45:44
tags: [postgresql]
---

**查询时区**

```sql
select * from pg_timezone_names;
```

**格式化时间**

```sql
-- 格式化当前时间
SELECT to_char(now() at time zone 'Asia/Shanghai', 'YYYY-MM-DD HH24:MI:SS+8:00');
-- 格式化指定时间
SELECT to_char(timestamp '2018-11-29 17:00:00' at time zone 'Asia/Shanghai', 'YYYY-MM-DD HH24:MI:SS+8:00');
```

**反格式化时间**

```sql
-- 获取当前时间的0点
select timestamptz(to_char(now() at time zone 'Asia/Shanghai', 'YYYY-MM-DD 00:00:00+8:00'));
-- 获取指定时间的0点
select timestamptz(to_char(timestamp '2018-11-29 17:00:00' at time zone 'Asia/Shanghai', 'YYYY-MM-DD 00:00:00+8:00'));
```

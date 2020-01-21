---
title: PostgreSQL日期时间
date: 2018-11-29 09:45:44
tags: [postgresql, sql]
---

**查询时区**

```sql
select * from pg_timezone_names;
```

**格式化时间**

```sql
-- 格式化当前时间
select to_char(now() at time zone 'Asia/Shanghai', 'YYYY-MM-DD HH24:MI:SS+8:00');
-- 格式化指定时间
select to_char(timestamp '2018-11-29 17:00:00' at time zone 'Asia/Shanghai', 'YYYY-MM-DD HH24:MI:SS+8:00');
```

**反格式化时间**

```sql
-- 获取当前时间的0点
select timestamptz(to_char(now() at time zone 'Asia/Shanghai', 'YYYY-MM-DD 00:00:00+8:00'));
-- 获取指定时间的0点
select timestamptz(to_char(timestamp '2018-11-29 17:00:00' at time zone 'Asia/Shanghai', 'YYYY-MM-DD 00:00:00+8:00'));
```

**日期计算**

```sql
select now() - '1 month'::interval;     -- 1月前
select now() + '1 hour'::interval;      -- 1小时后
select now() + '1 year 1 month 1 day 1 hour 1 min 1 sec'::interval;     -- 1年1月1日1小时1分钟1秒后
```

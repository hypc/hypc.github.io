---
title: PostgreSQL 'NOT IN' 子查询
date: 2020-08-21 15:03:06
tags: [postgresql]
---

今天遇到一个奇怪的问题，就是在使用`NOT IN`操作时后面跟一个子查询，发现始终查询不到结果，
但是使用`IN`操作能够查询到结果。SQL语句如下：

```sql
SELECT * FROM parking_records
WHERE parking_id = '1'
AND car_id NOT IN (SELECT car_id FROM users);
```

查询相关资料后发现，在使用`NOT IN`操作时，你需要确认`values`中不能有`NULL`值，修改后的SQL语句如下：

```sql
SELECT * FROM parking_records
WHERE parking_id = '1'
AND car_id NOT IN (
    SELECT car_id FROM users
    WHERE car_id IS NOT NULL    -- 增加非NULL判断
);
```

---
title: 在alpine容器中使用定时任务
date: 2018-07-25 22:05:06
tags: [docker, alpine]
---

首先alpine内嵌的是BusyBox，使用alpine的crontab实际就是使用BusyBox的crond服务。

配置文件的位置：`/var/spool/cron/crontabs/root`，原始内容是：

```crontab
# do daily/weekly/monthly maintenance
# min   hour    day     month   weekday command
*/15    *       *       *       *       run-parts /etc/periodic/15min
0       *       *       *       *       run-parts /etc/periodic/hourly
0       2       *       *       *       run-parts /etc/periodic/daily
0       3       *       *       6       run-parts /etc/periodic/weekly
0       5       1       *       *       run-parts /etc/periodic/monthly
```

可以在后面添加自己的定时任务：

```crontab
# do daily/weekly/monthly maintenance
# min   hour    day     month   weekday command
*/15    *       *       *       *       run-parts /etc/periodic/15min
0       *       *       *       *       run-parts /etc/periodic/hourly
0       2       *       *       *       run-parts /etc/periodic/daily
0       3       *       *       6       run-parts /etc/periodic/weekly
0       5       1       *       *       run-parts /etc/periodic/monthly
*       *       *       *       *       echo 'test'
```

**使用docker-compose部署定时任务**

```yaml
version: "2"
services:
    crond:
        image: alpine:3.4
        volumes:
            - ./crontabs/root:/var/spool/cron/crontabs/root
        command: crond -f
```

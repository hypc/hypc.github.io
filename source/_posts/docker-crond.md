---
title: Docker中运行定时任务
date: 2018-07-06 14:34:29
tags: [docker]
---

## 直接使用cron启动

**Dockerfile**:

```dockerfile
FROM ubuntu:14.04

RUN apt-get update && apt-get install -y cron

ADD . /app
WORKDIW /app

RUN echo '* * * * * root bash -c "/app/cron.sh"' >> /etc/crontab

CMD ["cron", "-f"]
```

### 注意

有时候在docker容器中启动定时任务的时候可能取不到环境变量，此时可以这样启动定时任务：

```bash
env >> /etc/environment && cron -f
```

## 使用supervisor管理

**cron.conf**:

```
[program:cron]
command=bash -c "cron -f"
user=root
autostart=true
autorestart=true
redirect_stderr=true
```

**Dockerfile**:

```dockerfile
FROM ubuntu:14.04

RUN apt-get update && apt-get install -y supervisor cron

ADD . /app
WORKDIW /app

RUN echo '* * * * * root bash -c "/app/cron.sh"' >> /etc/crontab
RUN cat cron.conf > /etc/supervisor/conf.d/cron.conf

CMD ["supervisor", "-n"]
```

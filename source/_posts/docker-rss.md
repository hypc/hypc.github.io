---
title: 使用Docker部署RSSHub以及FreshRSS
date: 2021-04-06 11:01:18
tags: [docker, rss]
---

[RSSHub][]是一个开源、简单易用、易于扩展的RSS生成器，可以给任何奇奇怪怪的内容生成RSS订阅源。

[FreshRSS][]是开源免费RSS订阅工具，功能设置上类似于`Google Reader`，支持快捷键操作，多用户，Ajax加载，数据导入与导出以及统计数据。

[RSSHub]: https://docs.rsshub.app/
[FreshRSS]: https://github.com/FreshRSS/FreshRSS

这里直接使用`docker-compose`一键化部署：

```yaml
version: '3.7'

services:
  freshrss:
    image: freshrss/freshrss
    labels:
      - traefik.http.routers.freshrss.rule=Host(`freshrss.yourdomain.com`)
      - traefik.http.routers.freshrss.entrypoints=websecure
      - traefik.http.routers.freshrss.service=freshrss
      - traefik.http.services.freshrss.loadbalancer.server.port=80
    environment:
      - TZ=Asia/Shanghai
      - CRON_MIN=*/20
  rsshub:   # port: 1200
    image: diygod/rsshub
    restart: always
    labels:
      - traefik.enable=false
    environment:
      - NODE_ENV=production
      - PUPPETEER_WS_ENDPOINT=ws://browserless-chrome:3000
  browserless-chrome:   # port: 3000
    image: browserless/chrome
    restart: always
    labels:
      - traefik.enable=false
```

<!--more-->

使用浏览器打开 https://freshrss.yourdomain.com ：

<video controls autoplay src="/images/docker-rss.mp4" style="max-width: 80%;"></video>

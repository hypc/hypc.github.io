---
title: 【BUG】NGINX+GUNICORN+DJANGO出现400错误
date: 2018-07-24 20:13:00
tags: [nginx, python, django, gunicorn]
---

配置Nginx+Gunicorn+Django时，发现所有请求都是返回`Bad Request (400)`。

1. django的`settings.py`配置`ALLOWED_HOSTS = [*]`

2. 配置nginx反向代理`proxy_set_header Host $host;`

    ```nginx
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $remote_addr;
    client_max_body_size 20m;
    client_body_buffer_size 256k;
    proxy_connect_timeout 90;
    proxy_send_timeout 90;
    proxy_read_timeout 90;
    proxy_buffer_size 128k;
    proxy_buffers 4 64k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size 128k;
    ```

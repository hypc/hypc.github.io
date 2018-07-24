---
title: nginx反向代理重定向
date: 2018-07-24 20:07:29
tags: [nginx]
---

当访问`http://example.com/a/abc.html`，
实际上方向代理到后端服务器`http://10.0.x.x:8080/b/abc.html`，
那么可以按照以下方式进行配置：

```nginx
server {
    listen      80;
    server_name example.com;

    rewrite  /a$  /a/  permanent;   # 防止访问`/a`时返回404
    location ~ /a/(?<section>.*) {
        proxy_pass  http://10.0.x.x:8080/b/$section;
    }
}
```

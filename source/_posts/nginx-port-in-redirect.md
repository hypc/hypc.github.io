---
title: nginx的port_in_redirect配置
date: 2021-04-21 18:14:09
tags: [nginx]
---

当nginx监听的端口与实际访问端口不一致时（如多层反向代理、或nat转发等），这时候通常需要关闭`port_in_redirect`选项。

> port_in_redirect: 默认开启状态。启用或禁用在nginx发出的绝对重定向中指定端口。

```nginx
server {
    listen      8080;

    location /public/ {
        root        html;
        index       index.html index.htm;
        port_in_redirect    off;
        proxy_buffering     off;
    }
}
```

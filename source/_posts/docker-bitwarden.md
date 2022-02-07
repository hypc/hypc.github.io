---
title: 使用Docker搭建Bitwarden服务
date: 2020-12-06 14:28:41
tags: [docker, bitwarden]
---

[Bitwarden][]是一款开源的密码管理工具。

[Bitwarden]: https://bitwarden.com/

```yaml
version: '3.7'

services:
  bitwarden:
    image: bitwardenrs/server
    restart: always
    portss:
      - 80:80
    volumes:
      - data:/data/

volumes:
  data:
```

注意：在使用浏览器插件的时候需要必须配置ssl证书。

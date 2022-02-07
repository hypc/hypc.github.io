---
title: 使用Docker部署Odoo服务
date: 2021-03-04 14:44:13
tags: [docker, erp]
---

[Odoo][]是基于Python写的一系列开源商业ERP系统，前身是OpenERP。

[Odoo]: https://www.odoo.com/

```yaml
version: '3.7'

services:
  odoo:
    image: odoo
    restart: always
    labels:
      - traefik.http.routers.odoo.rule=Host(`odoo.yourdomain.com`)
      - traefik.http.routers.odoo.entrypoints=websecure
      - traefik.http.routers.odoo.service=odoo
      - traefik.http.services.odoo.loadbalancer.server.port=8069
    environment:
      - HOST=postgres
      - PORT=5432
      - USER=odoo
      - PASSWORD=odoo
    volumes:
      - data:/var/lib/odoo
      - addons:/mnt/extra-addons

volumes:
  data:
  addons:
```

值得注意的是，服务初始化的时候无需创建数据库，它会在后续的初始化过程中自动创建。

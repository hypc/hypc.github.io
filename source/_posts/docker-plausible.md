---
title: 使用Docker搭建Plausible服务
date: 2021-06-20 17:17:17
tags: [docker, plausible]
---

[Plausible][]是一种轻量级、开源的网站分析工具。

[Plausible]: https://plausible.io/

{% tabs %}
<!-- tab plausible.env -->
```ini
ADMIN_USER_NAME=admin
ADMIN_USER_EMAIL=admin@yourdomain.com
ADMIN_USER_PWD=QAGvniNx7pEwYBYoHjD9AW3y8PmrQbJT

BASE_URL=https://plausible.yourdomain.com
SECRET_KEY_BASE=BwX9gsbCdYkMfRkaNecRTN4LVA4a8RyUjiENT5J93Dkmrg8LdZyabKBDDGqD43Wr
DATABASE_URL=postgres://postgres:postgres@plausible-postgres:5432/plausible
CLICKHOUSE_DATABASE_URL=http://plausible-clickhouse:8123/plausible

SMTP_HOST_ADDR=plausible-smtp
SMTP_HOST_PORT=25
```

更多配置参考：https://plausible.io/docs/self-hosting-configuration
<!-- endtab -->

<!-- tab docker-compose.yaml -->
```yaml
version: '3.7'

services:
  plausible:
    image: plausible/analytics
    restart: always
    labels:
      - traefik.http.routers.plausible.rule=Host(`plausible.yourdomain.com`)
      - traefik.http.routers.plausible.entrypoints=websecure
      - traefik.http.routers.plausible.service=plausible
      - traefik.http.services.plausible.loadbalancer.server.port=8000
    env_file: plausible.env
    environment:
      - GEOLITE2_COUNTRY_DB=/geoip/GeoLite2-Country.mmdb
    volumes:
      - geoip:/geoip:ro
  plausible-init:
    image: plausible/analytics
    restart: on-failure
    labels:
      - traefik.enable=false
    env_file: plausible.env
    command: sh -c '/entrypoint.sh db createdb && /entrypoint.sh db migrate && /entrypoint.sh db init-admin'
  plausible-postgres:
    image: postgres:12-alpine
    restart: always
    labels:
      - traefik.enable=false
    environment:
      - POSTGRES_PASSWORD=postgres
    volumes:
      - postgres:/var/lib/postgresql/data
  plausible-clickhouse:
    image: yandex/clickhouse-server:21.3    # 不能高于21.3
    restart: always
    labels:
      - traefik.enable=false
    volumes:
      - clickhouse:/var/lib/clickhouse
  plausible-smtp:
    image: tianon/exim4
    restart: always
    labels:
      - traefik.enable=false
  plausible-geoipupdate:
    image: maxmindinc/geoipupdate
    labels:
      - traefik.enable=false
    environment:
      - GEOIPUPDATE_ACCOUNT_ID=***      # https://dev.maxmind.com/geoip/geolite2-free-geolocation-data
      - GEOIPUPDATE_LICENSE_KEY=***
      - "GEOIPUPDATE_EDITION_IDS=GeoLite2-ASN GeoLite2-City GeoLite2-Country"
      - GEOIPUPDATE_FREQUENCY=7
    volumes:
      - geoip:/usr/share/GeoIP

volumes:
  postgres:
  clickhouse:
  geoip:
```
<!-- endtab -->
{% endtabs %}

1. start smtp & postgres & clockhouse & geoipupdate

    ```bash
    docker-compose up -d plausible-smtp plausible-postgres plausible-clickhouse plausible-geoipupdate
    docker-compose exec plausible-clickhouse clickhouse-client --query 'CREATE DATABASE IF NOT EXISTS plausible;'
    docker-compose exec plausible-clickhouse clickhouse-client --query 'SHOW DATABASES;'
    ```

2. plausible init

    ```bash
    docker-compose up plausible-init
    ```

3. run plausible

    ```bash
    docker-compose up -d plausible
    ```

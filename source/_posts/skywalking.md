---
title: SkyWalking
date: 2021-08-21 13:29:42
tags: [nginx, skywalking, openresty, opentracing]
---

[SkyWalking][] 是一款分布式链路追踪系统。

[OpenResty][] 是一个基于 [Nginx][] 与 Lua 的高性能 Web 平台，
其内部集成了大量精良的 Lua 库、第三方模块以及大多数的依赖项。
用于方便地搭建能够处理超高并发、扩展性极高的动态 Web 应用、Web 服务和动态网关。

[SkyWalking]: https://skywalking.apache.org/
[OpenResty]: https://openresty.org/
[Nginx]: https://openresty.org/cn/nginx.html

<!--more-->

## 运行 SkyWalking 服务

```yaml
version: '3.7'

services:
  skywalking-oap:
    image: apache/skywalking-oap-server:8.7.0-es7
    restart: always
    labels:
      - traefik.enable=false
    environment:
      - SW_STORAGE=elasticsearch7
      - SW_STORAGE_ES_CLUSTER_NODES=skywalking-elasticsearch:9200
      - TZ=Asia/Shanghai
  skywalking-ui:
    image: apache/skywalking-ui:8.7.0
    restart: always
    labels:
      - traefik.http.routers.skywalking.rule=Host(`skywalking.yourdomain.com`)
      - traefik.http.routers.skywalking.entrypoints=websecure
      - traefik.http.routers.skywalking.service=skywalking
      - traefik.http.services.skywalking.loadbalancer.server.port=8080
      - traefik.http.routers.skywalking.middlewares=try-file
      - traefik.http.middlewares.try-file.errors.status=404
      - traefik.http.middlewares.try-file.errors.service=skywalking
      - traefik.http.middlewares.try-file.errors.query=/index.html
    environment:
      - SW_OAP_ADDRESS=http://skywalking-oap:12800
  skywalking-elasticsearch:
    image: elasticsearch:7.14.1
    restart: always
    labels:
      - traefik.enable=false
    environment:
      - XPACK_LICENSE_SELF_GENERATED_TYPE=basic
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx2048m
    volumes:
      - data:/usr/share/elasticsearch/data

volumes:
  data:
```

## 部署 OpenResty 服务

### 准备 `skywalking-nginx-lua` 源码

skywalking-nginx-lua: https://github.com/apache/skywalking-nginx-lua

### 编写 `nginx.conf` 配置文件

```nginx
# nginx.conf  --  docker-openresty
#
# This file is installed to:
#   `/usr/local/openresty/nginx/conf/nginx.conf`
# and is the file loaded by nginx at startup,
# unless the user specifies otherwise.
#
# It tracks the upstream OpenResty's `nginx.conf`, but removes the `server`
# section and adds this directive:
#     `include /etc/nginx/conf.d/*.conf;`
#
# The `docker-openresty` file `nginx.vh.default.conf` is copied to
# `/etc/nginx/conf.d/default.conf`.  It contains the `server section
# of the upstream `nginx.conf`.
#
# See https://github.com/openresty/docker-openresty/blob/master/README.md#nginx-config-files
#

#user  nobody;
#worker_processes 1;

# Enables the use of JIT for regular expressions to speed-up their processing.
pcre_jit on;



#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    # Enables or disables the use of underscores in client request header fields.
    # When the use of underscores is disabled, request header fields whose names contain underscores are marked as invalid and become subject to the ignore_invalid_headers directive.
    # underscores_in_headers off;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

        # Log in JSON Format
        # log_format nginxlog_json escape=json '{ "timestamp": "$time_iso8601", '
        # '"remote_addr": "$remote_addr", '
        #  '"body_bytes_sent": $body_bytes_sent, '
        #  '"request_time": $request_time, '
        #  '"response_status": $status, '
        #  '"request": "$request", '
        #  '"request_method": "$request_method", '
        #  '"host": "$host",'
        #  '"upstream_addr": "$upstream_addr",'
        #  '"http_x_forwarded_for": "$http_x_forwarded_for",'
        #  '"http_referrer": "$http_referer", '
        #  '"http_user_agent": "$http_user_agent", '
        #  '"http_version": "$server_protocol", '
        #  '"nginx_access": true }';
        # access_log /dev/stdout nginxlog_json;

    # See Move default writable paths to a dedicated directory (#119)
    # https://github.com/openresty/docker-openresty/issues/119
    client_body_temp_path /var/run/openresty/nginx-client-body;
    proxy_temp_path       /var/run/openresty/nginx-proxy;
    fastcgi_temp_path     /var/run/openresty/nginx-fastcgi;
    uwsgi_temp_path       /var/run/openresty/nginx-uwsgi;
    scgi_temp_path        /var/run/openresty/nginx-scgi;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    # Don't reveal OpenResty version to clients.
    # server_tokens off;


    lua_package_path "/skywalking-nginx-lua/lib/?.lua;;";

    # Buffer represents the register inform and the queue of the finished segment
    lua_shared_dict tracing_buffer 100m;

    # Init is the timer setter and keeper
    # Setup an infinite loop timer to do register and trace report.
    init_worker_by_lua_block {
        local metadata_buffer = ngx.shared.tracing_buffer

        -- Set service name
        metadata_buffer:set('serviceName', '192.168.56.20')
        -- Instance means the number of Nginx deployment, does not mean the worker instances
        metadata_buffer:set('serviceInstanceName', '192.168.56.20')
        -- type 'boolean', mark the entrySpan include host/domain
        metadata_buffer:set('includeHostInEntrySpan', true)

        -- set random seed
        require("skywalking.util").set_randomseed()
        require("skywalking.client"):startBackendTimer("http://skywalking-oap:12800")

        -- If there is a bug of this `tablepool` implementation, we can
        -- disable it in this way
        -- require("skywalking.util").disable_tablepool()

        skywalking_tracer = require("skywalking.tracer")
    }

    server {
        listen      80;
        server_name _;

        location / {
            rewrite_by_lua_block {
                ------------------------------------------------------
                -- NOTICE, this should be changed manually
                -- This variable represents the upstream logic address
                -- Please set them as service logic name or DNS name
                --
                -- Currently, we can not have the upstream real network address
                ------------------------------------------------------
                skywalking_tracer:start(ngx.var.host)
                -- If you want correlation custom data to the downstream service
                -- skywalking_tracer:start(ngx.var.host, {custom = "custom_value"})
            }

            proxy_pass http://10.1.50.103:8080/;
            proxy_set_header   Host              $host;
            proxy_set_header   X-Real-IP         $remote_addr;
            proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto https;

            body_filter_by_lua_block {
                if ngx.arg[2] then
                    skywalking_tracer:finish()
                end
            }

            log_by_lua_block {
                skywalking_tracer:prepareForReport()
            }
        }
    }
}
```

### 部署 OpenResty 服务

```yaml
version: '3.7'

services:
  nginx:
    image: openresty/openresty:alpine
    restart: always
    ports:
      - 21423:80
    volumes:
      - ./nginx.conf:/usr/local/openresty/nginx/conf/nginx.conf
      - ./skywalking-nginx-lua:/skywalking-nginx-lua
```

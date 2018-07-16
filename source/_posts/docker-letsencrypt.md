---
title: Deploying Nginx and Letsencrypt in Docker
date: 2018-07-16 22:07:23
tags: [docker, nginx, letsencrypt, ssl]
---

## 准备

1. 安装Docker
2. 安装Docker-compose
3. 拉取镜像：`nginx`、`certbot/certbot`
4. 域名`your.domain.com`

## 部署

### 从nginx镜像中复制`nginx.conf`文件

```bash
docker run --name tmp-nginx-container -d nginx
docker cp tmp-nginx-container:/etc/nginx/nginx.conf `pwd`/nginx.conf
docker rm -f tmp-nginx-container
```

### 配置your.domain.com的http服务

```nginx
server {
    listen          80;
    server_name     your.domain.com;

    location ^~ /.well-known/acme-challenge/ {
        root        /usr/share/nginx/html;
    }
    location / {
        rewrite     ^/(.*)$      https://$server_name/$1     permanent;
    }
}
```

### 配置docker-compose.yaml文件

```yaml
version: "2"
services:
    nginx:
        image: nginx
        restart: always
        ports:
            - 80:80
            - 443:443
        volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf
            - ./conf.d:/etc/nginx/conf.d
            - ./letsencrypt:/etc/letsencrypt
            - ./html:/usr/share/nginx/html
```

### 启动nginx服务

```bash
docker-compose up -d
```

### 生成Letsencrypt证书

```bash
docker run -it \
    -v `pwd`/letsencrypt:/etc/letsencrypt \
    -v `pwd`/html:/usr/share/nginx/html \
    certbot/certbot \
    certonly --webroot -w /usr/share/nginx/html/ \
    -d your.domain.com
```

### 配置your.domain.com的https服务

```nginx
server {
    listen          443;
    server_name     your.domain.com;

    ssl on;
    ssl_certificate     /etc/letsencrypt/live/your.domain.com/fullchain.pem;
    ssl_certificate_key     /etc/letsencrypt/live/your.domain.com/privkey.pem;
    ssl_trusted_certificate     /etc/letsencrypt/live/your.domain.com/chain.pem;

    location ^~ /.well-known/acme-challenge/ {
        root        /usr/share/nginx/html;
    }
    location / {
        root        /usr/share/nginx/html;
    }
}
```

### 重启nginx服务

```bash
docker-compose exec nginx nginx -s reload
```

## 刷新Letsencrypt证书

```bash
docker run -it \
    -v `pwd`/letsencrypt:/etc/letsencrypt \
    -v `pwd`/html:/usr/share/nginx/html \
    certbot/certbot \
    renew
```

建议每隔一个月刷新一次证书，将其写入定时任务。

---
title: 使用Docker部署thinkphp项目
date: 2018-10-19 08:33:06
tags: [docker, php, thinkphp]
---

使用Docker部署thinkphp，需要注意几件事：

1. thinkphp的入口文件在public目录下，所以需要将`VirtualHost`的`DocumentRoot`指向public目录；
2. thinkphp需要设置伪静态，Apache需要加载`mod_rewrite.so`模块；
3. 下载相应版本的thinkphp放到项目根目录下。

```dockerfile
FROM php:5.6-apache

ENV THINKPHP_VERSION=5.0.21

RUN ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load \
    && sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf \
    && sed -i 's/\/var\/www\/html/\/var\/www\/html\/public/g' /etc/apache2/sites-enabled/000-default.conf
RUN curl -OL https://github.com/top-think/framework/archive/v$THINKPHP_VERSION.tar.gz \
    && tar xvf v$THINKPHP_VERSION.tar.gz && mv framework-$THINKPHP_VERSION thinkphp

ADD . /var/www/html/
```

如果在项目中用到了扩展，使用`docker-php-ext-install`命令安装相应的扩展：

```dockerfile
# 安装postgresql扩展
RUN apt-get update && apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql

# 安装mysql扩展
RUN docker-php-ext-install pdo pdo_mysql
```

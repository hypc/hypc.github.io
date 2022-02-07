---
title: 使用Docker部署VtigerCRM
date: 2020-10-02 20:47:10
tags: [docker, vtigercrm, crm]
---

[VtigerCRM][]是一套基于Web以销售能力自动化(SFA)为主的客户关系管理系统(CRM)。它基于是[SugarCRM][]专业版(SPL1.1.2)开发的一个衍生版本。

[VtigerCRM][]系统是一个适合中小企业的客户管理工具，帮助管理公司业务，从市场、销售、采购、库存、客服等全程跟踪客户，
最大可能获得订单，提高客户满意度。比较适合销售团队，贸易公司，服务型企业使用。部分作用如下：

* 统一记录与管理客户资料，不再担心资料丢失
* 随时随地搜索查找客户资料，及时联系客户
* 自动化的统计分析您的客户信息、销售情况等
* 全面掌握公司业务信息，避免人员离职等导致客户流失
* 通过CRM系统的工作流使公司业务流程自动化

[VtigerCRM]: https://www.vtiger.com/
[SugarCRM]: https://www.sugarcrm.com/

<!--more-->

## 启动服务

### 编写Dockerfile文件

```dockerfile
FROM php:7.4-apache

RUN apt-get update && apt-get install -y libpng-dev libjpeg-dev libkrb5-dev libssl-dev libc-client2007e-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install gd mysqli \
    && docker-php-ext-configure imap --with-imap-ssl --with-kerberos \
    && docker-php-ext-install imap opcache \
    && rm -rf /var/lib/apt/lists/*

# setting the reccomended for opcache
# https://secure.php.net/manual/en/opcache.installation.php
RUN { \
        echo 'opcache.memory_consumption=128'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=4000'; \
        echo 'opcache.revalidate_freq=60'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# setting the recommended for vtiger
RUN { \
        echo 'display_errors=On'; \
        echo 'max_execution_time=0'; \
        echo 'error_reporting=E_WARNING & ~E_NOTICE & ~E_DEPRECATED'; \
        echo 'log_errors=Off'; \
        echo 'short_open_tag=On'; \
    } > /usr/local/etc/php/conf.d/vtiger-recommended.ini

RUN cd /var/www/ && curl -o vtigercrm.tar.gz -SL https://sourceforge.net/projects/vtigercrm/files/latest/download \
    && tar xvf vtigercrm.tar.gz \
    && chown -R www-data:www-data vtigercrm \
    && chmod -R 775 vtigercrm \
    && sed -i "s/\$defaultParameters\['db_hostname'\]/getenv('DB_HOSTNAME')/" vtigercrm/modules/Install/views/Index.php \
    && sed -i "s/\$defaultParameters\['db_username'\]/getenv('DB_USERNAME')/" vtigercrm/modules/Install/views/Index.php \
    && sed -i "s/\$defaultParameters\['db_password'\]/getenv('DB_PASSWORD')/" vtigercrm/modules/Install/views/Index.php \
    && sed -i "s/\$defaultParameters\['db_name'\]/getenv('DB_NAME')/" vtigercrm/modules/Install/views/Index.php \
    && rm -rf html/ && mv vtigercrm html

VOLUME /var/www/html/storage
CMD ["apache2-foreground"]
```

### 编写docker-compose.yaml文件

```yaml
version: '3.7'

services:
  vtigercrm:
    build: .
    container_name: vtigercrm
    depends_on:
      - vtigercrm-db
    restart: always
    ports:
      - 23867:80
    environment:
      DB_HOSTNAME: vtigercrm-db
      DB_USERNAME: vtigercrm
      DB_PASSWORD: vtigercrm
      DB_NAME: vtigercrm
    volumes:
      - ./storage:/var/www/html/storage
  vtigercrm-db:
    image: mysql:8.0
    container_name: vtigercrm-db
    command: --default-authentication-plugin=mysql_native_password --sql_mode=""
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 160544A82BF449C6899A59A1E8FD6D2C
      MYSQL_DATABASE: vtigercrm
      MYSQL_USER: vtigercrm
      MYSQL_PASSWORD: vtigercrm
    volumes:
      - ./data:/var/lib/mysql
```

### 启动服务

```bash
docker-compose up -d --build
```

## VtigerCRM初始化

打开网页 http://localhost/ ，按照提示进行初始化操作：

<video controls autoplay src="/images/docker-vtigercrm.mp4"></video>

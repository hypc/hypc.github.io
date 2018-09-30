---
title: 使用Docker部署Moodle系统
date: 2018-09-20 20:46:29
tags: [docker, moodle]
---

## Moodle简介

[Moodle][]是一个开源课程管理系统（CMS），也被称为学习管理系统（LMS）或虚拟学习环境（VLE）。

[Moodle][]平台界面简单、精巧。使用者可以根据需要随时调整界面，增减内容。
课程列表显示了服务器上每门课程的描述，包括是否允许访客使用，访问者可以对课程进行分类和搜索，按自己的需要学习课程。

[Moodle][]平台还具有兼容和易用性。可以几乎在任何支持PHP的平台上安装，安装过程简单。只需要一个数据库（并且可以共享）。
它具有全面的数据库抽象层，几乎支持所有的主流数据库（除了初始表定义）。
利用[Moodle][]，现今主要的媒体文件都可以进行传送，这使可以利用的资源极大丰富。
在对媒体资源进行编辑时，利用的是用所见即所得的编辑器，这使得使用者无需经过专业培训，就能掌握Moodle的基本操作与编辑。
[Moodle][]注重全面的安全性，所有的表单都被检查，数据都被校验，cookie是被加密的。
用户注册时，通过电子邮件进行首次登陆，且同一个邮件地址不能在同一门课程中进行重复注册，所有这些，都使得[Moodle][]的安全性得到了加强。
目前，[Moodle][]项目仍然在不断的开发与完善中。

[Moodle][]是B/S模式的应用程序，但是一般而言，只适合于中小型学校。

<!--more-->

## docker-compose配置文件

```yaml
version: '3'
services:
    moodle:
        image: jhardison/moodle:v3.5
        container_name: moodle
        restart: always
        ports:
            - 8082:80
        environment:
            - MOODLE_URL=http://10.1.50.112:8082
            - DB_PORT_3306_TCP_ADDR=moodle_db
            - DB_ENV_MYSQL_DATABASE=moodle
            - DB_ENV_MYSQL_USER=moodle
            - DB_ENV_MYSQL_PASSWORD=moodle
        volumes:
            - ./data:/var/moodledata
        depends_on:
            - moodle_db
    moodle_db:
        image: mysql:5.7
        container_name: moodle_db
        restart: always
        environment:
            - MYSQL_DATABASE=moodle
            - MYSQL_ROOT_PASSWORD=moodle
            - MYSQL_USER=moodle
            - MYSQL_PASSWORD=moodle
        volumes:
            - ./db:/var/lib/mysql
networks:
    default:
        driver: bridge
        ipam:
            driver: default
            config:
                 - subnet: 172.27.3.0/24
```

**注意**：需要先启动数据库，并等数据库初始化完成之后再启动moodle服务。

[Moodle]: https://moodle.org/

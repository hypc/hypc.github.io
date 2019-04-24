---
title: 使用Docker部署NextCloud和Collabora
date: 2019-04-22 14:54:45
tags: [docker, nextcloud, collaboraoffice, office]
---

**[NextCloud][nextcloud]**是一款用于自建私有网盘的云存储开源软件，采用PHP+MySQL开发，
功能类似百度云盘，提供了PC、IOS和Android三个同步客户端支持多种设备访问，
用户可以很方便地与服务器上存储的文件、日程安排、通讯录、书签等重要数据保持同步，
还支持其他同步来源：Amazon S3、Dropbox、FTP、Google Drive、OpenStack Object Storage、SMB、WebDAV、SFTP。

**[Collabora][collabora]**是一个功能强大的基于libreoffice的在线办公室，
它支持所有主要文档、电子表格和演示文件格式，您可以将这些格式集成到自己的基础结构中。
主要功能是协同编辑和卓越的Office文件格式支持。

<!--more-->

## 部署服务

```yaml
version: '2'
services:
    nextcloud:
        image: nextcloud
        container_name: nextcloud
        restart: always
        ports:
            - 8087:80
        volumes:
            - ./data:/var/www/html
    collabora:
        image: collabora/code
        container_name: collabora
        restart: always
        ports:
            - 9980:9980
        environment:
            - extra_params=--o:ssl.enable=false
```

更详细的配置参考：[NextCloud Image][nextcloud_image]、[Collabora Image][collabora_image]。

## 配置

服务启动之后，浏览器打开`http://<your-ip>:8087/`，按照提示配置好NextCloud服务器参数，
然后安装`Collabora Online`插件，接着在设置里面找到`在线协作`，
在`URL (and Port) of Collabora Online-server`里面配置`http://<your-ip>:9980/`。


[nextcloud]: https://nextcloud.com/
[collabora]: https://www.collabora.com/
[nextcloud_image]: https://hub.docker.com/_/nextcloud
[collabora_image]: https://hub.docker.com/r/collabora/code

---
title: 使用Docker部署NextCloud和OnlyOffice
date: 2019-04-02 16:11:06
tags: [docker, nextcloud, onlyoffice, office]
---

**[NextCloud][nextcloud]**是一款用于自建私有网盘的云存储开源软件，采用PHP+MySQL开发，
功能类似百度云盘，提供了PC、IOS和Android三个同步客户端支持多种设备访问，
用户可以很方便地与服务器上存储的文件、日程安排、通讯录、书签等重要数据保持同步，
还支持其他同步来源：Amazon S3、Dropbox、FTP、Google Drive、OpenStack Object Storage、SMB、WebDAV、SFTP。

**[ONLYOFFICE][onlyoffice]**是一款集成了文档、电子邮件、事件、任务和客户关系管理工具的开源在线办公套件。
其文档管理功能实现了文档的在线编辑、在线预览和协同管理，可用于替代Office365或Google docs。
另外，它还提供了CRM、项目管理等功能，非常合适作为企业内部的全员协作Office系统。

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
    onlyoffice:
        image: onlyoffice/documentserver
        container_name: onlyoffice
        restart: always
        ports:
            - 8088:80
```

更详细的配置参考：[NextCloud Image][nextcloud_image]、[ONLYOFFICE Image][onlyoffice_image]。

## 配置

服务启动之后，浏览器打开`http://<your-ip>:8087/`，按照提示配置好NextCloud服务器参数，
然后安装`ONLYOFFICE`插件，接着在设置里面找到`ONLYOFFICE`，
在`Document Editing Service address`里面配置`http://<your-ip>:8088/`。


[nextcloud]: https://nextcloud.com/
[onlyoffice]: https://www.onlyoffice.com/
[nextcloud_image]: https://hub.docker.com/_/nextcloud
[onlyoffice_image]: https://hub.docker.com/r/onlyoffice/documentserver

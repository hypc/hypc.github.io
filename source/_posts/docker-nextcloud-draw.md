---
title: 使用Docker部署NextCloud和draw.io
date: 2019-05-05 10:43:09
tags: [docker, nextcloud]
---

**[NextCloud][nextcloud]**是一款用于自建私有网盘的云存储开源软件，采用PHP+MySQL开发，
功能类似百度云盘，提供了PC、IOS和Android三个同步客户端支持多种设备访问，
用户可以很方便地与服务器上存储的文件、日程安排、通讯录、书签等重要数据保持同步，
还支持其他同步来源：Amazon S3、Dropbox、FTP、Google Drive、OpenStack Object Storage、SMB、WebDAV、SFTP。

**[draw.io][]**是一个强大简洁的在线的绘图网站，支持流程图，UML图，架构图，原型图等图标。
支持Github，Google Drive, One drive等网盘同步，并且永久免费。
如果觉得使用Web版不方便，draw.io 也提供了多平台的离线桌面版可供下载。

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
    draw:
        image: fjudith/draw.io:10.6.3-alpine
        container_name: draw
        restart: always
        ports:
            - 8089:8080
```

更详细的配置参考：[NextCloud Image][nextcloud_image]、[draw.io][draw.io_image]。

## 配置

服务启动之后，浏览器打开`http://<your-ip>:8087/`，按照提示配置好NextCloud服务器参数，
然后安装`Draw.io`插件，接着在设置里面找到`其他设置`，在`Draw.io URL`里面配置`http://<your-ip>:8089/`。


[nextcloud]: https://nextcloud.com/
[draw.io]: https://www.draw.io/
[nextcloud_image]: https://hub.docker.com/_/nextcloud
[draw.io_image]: https://hub.docker.com/r/fjudith/draw.io

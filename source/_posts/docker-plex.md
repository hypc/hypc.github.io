---
title: Deploying Plex in Docker
date: 2019-04-15 08:20:20
tags: [docker, plex]
---

[Plex][]是一套媒体播放器及媒体服务器软件，由于Plex Media Server和Plex Media Player组成，
主要功能是存储+索引+转码+在线播放。Plex Media Server不是简单地帮你存储分类影音文件，
它还能分析影片的信息从而从IMDB等数据库补全影片介绍等信息，进行索引以方便搜索。

[Plex][]可用于Windows、Android、Linux、OS X、FreeBSD和XBox，PS，各种TV，树莓派等，可以说是全平台通吃，
甚至与Bitcasa、Box和Dropbox等云服务兼容，Plex支持在线格式转换，支持将视频、音乐等各类文件转化为流至移动设备、智能电视和电子媒体播放器上。

## 部署服务

```yaml
version: '2'
services:
    plex:
        image: plexinc/pms-docker
        container_name: plex
        restart: always
        network_mode: host
        environment:
            - TZ=Asia/Shanghai
            - ADVERTISE_IP=http://192.168.31.160:32400/
        volumes:
            - ./media:/data
```

[Plex]: https://www.plex.tv/

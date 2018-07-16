---
title: Installing IntelliJ IDEA License Server in Docker
date: 2018-07-16 22:06:02
tags: [docker]
---

```bash
curl -o IntelliJIDEALicenseServer "http://home.ustc.edu.cn/~mmmwhy/86"
chmod a+x IntelliJIDEALicenseServer

cat <<EOF>> docker-compose.yaml
version: "2"
services:
    IntelliJIDEALicenseServer:
        image: ubuntu:16.04
        restart: always
        ports:
            - 1234:1234
        volumes:
            - ./IntelliJIDEALicenseServer:/usr/bin/IntelliJIDEALicenseServer
        command: IntelliJIDEALicenseServer -p 1234 -u admin
EOF

docker-compose up -d
```

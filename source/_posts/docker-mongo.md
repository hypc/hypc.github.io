---
title: 使用Docker部署MongoDB
date: 2020-08-01 14:27:50
tags: [docker, mongo]
---

[MongoDB][]是一个免费的开源跨平台的面向文档的NoSQL数据库。

[MongoDB]: https://www.mongodb.com/
[mongo]: https://hub.docker.com/_/mongo
[mongo-express]: https://hub.docker.com/_/mongo-express

```yaml
version: '3.7'

services:
  mongo:
    image: mongo
    restart: always
    ports:
      - 27017:27017
    volumes:
      - ./data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: 1C570DED651F41ABB99286C14DA4AD4A
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: root
      ME_CONFIG_MONGODB_ADMINPASSWORD: 1C570DED651F41ABB99286C14DA4AD4A
```

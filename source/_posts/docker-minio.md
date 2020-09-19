---
title: 使用Docker部署MinIO
date: 2020-08-02 14:40:27
tags: [docker, minio]
---

[MinIO][]是一个高性能对象存储服务。它与`Amazon S3`云存储服务兼容。

[MinIO]: https://min.io/

```yaml
version: '3.7'

services:
  minio:
    image: minio/minio
    restart: always
    ports:
      - 9000:9000
    command: server /data
    volumes:
      - ./data:/data
    environment:
      - MINIO_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE
      - MINIO_SECRET_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

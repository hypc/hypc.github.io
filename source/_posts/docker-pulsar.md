---
title: 使用Docker部署Pulsar
date: 2021-04-13 19:10:23
tags: [docker, pulsar]
---

[Pulsar][]在很多情况下提供了比Kafka更快的吞吐量和更低的延迟，并为开发人员提供了一组兼容的API，让他们可以很轻松地从Kafka切换到Pulsar。

[Pulsar][]的最大优点在于它提供了比Apache Kafka更简单明了、更健壮的一系列操作功能，特别在解决可观察性、地域复制和多租户方面的问题。在运行大型Kafka集群方面感觉有困难的企业可以考虑转向使用Pulsar。

[Pulsar]: https://pulsar.apache.org/

```yaml
version: '3.7'

services:
  pulsar:
    image: apachepulsar/pulsar
    restart: always
    labels:
      - traefik.http.routers.pulsar.rule=Host(`pulsar.yourdomain.com`)
      - traefik.http.routers.pulsar.entrypoints=websecure
      - traefik.http.routers.pulsar.service=pulsar
      - traefik.http.services.pulsar.loadbalancer.server.port=8080
    command: bin/pulsar standalone
    volumes:
      - conf:/pulsar/conf
      - data:/pulsar/data

volumes:
  conf:
  data:
```

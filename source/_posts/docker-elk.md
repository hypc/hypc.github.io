---
title: 快速搭建ELK平台
date: 2020-10-10 12:31:46
tags: [docker, elk, elasticsearch, kibana, logstash]
---

* [Elasticsearch][]: 一个分布式、RESTful风格的搜索和数据分析引擎
* [Kibana][]: 一个免费且开放的用户界面，能够对数据进行可视化
* [Logstash][]: 服务器端数据处理管道，能够从多个来源采集数据，转换数据，然后将数据发送到指定的存储库中

[Elasticsearch]: https://www.elastic.co/cn/elasticsearch/
[Kibana]: https://www.elastic.co/cn/kibana/
[Logstash]: https://www.elastic.co/cn/logstash/

```yaml
version: '3.7'
services:
  kibana:
    image: kibana:7.9.2
    restart: always
    ports:
      - 5601:5601
    environment:
      ELASTICSEARCH_HOSTS: http://elasticsearch:9200
      ELASTICSEARCH_USERNAME: elastic
      ELASTICSEARCH_PASSWORD: EA238970DA384AA3A308FB692714AA1B
  elasticsearch:
    image: elasticsearch:7.9.2
    restart: always
    ports:
      - 9200:9200
      - 9300:9300
    environment:
      ELASTIC_PASSWORD: EA238970DA384AA3A308FB692714AA1B
      XPACK_LICENSE_SELF_GENERATED_TYPE: basic
      discovery.type: single-node
  logstash:
    image: logstash:7.9.2
    restart: always
    ports:
      - 5000:5000
      - 9600:9600
    environment:
      XPACK_MONITORING_ELASTICSEARCH_HOSTS: http://elasticsearch:9200
      XPACK_MONITORING_ELASTICSEARCH_USERNAME: elastic
      XPACK_MONITORING_ELASTICSEARCH_PASSWORD: EA238970DA384AA3A308FB692714AA1B
```

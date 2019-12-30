---
title: Kafka入门
date: 2019-11-15 14:42:42
tags: [kafka]
---

## Kafka简介

[Kafka][]是一个高吞吐量、分布式的发布-订阅消息系统。

[Kafka][]是一款开源的、轻量级的、分布式、可分区和具有复制备份的（Replicated）、
基于[ZooKeeper][]协调管理的分布式流平台的功能强大的消息系统。
与传统的消息系统相比，[Kafka][]能够很好地处理活跃的流数据，使得数据在各个子系统中高性能、低延迟地不停流转。

[Kafka][]作为一个分布式流处理平台，具备以下三个特性：

* 能够允许发布和订阅流数据
* 存储流数据时提供相应的容错机制
* 当流数据到达时能够被及时处理

[Kafka]: https://kafka.apache.org/
[ZooKeeper]: https://zookeeper.apache.org/

<!--more-->

## Kafka基本概念

### 主题

Kafka将一组消息抽象归纳为一个主题（Topic），也就是说，一个主题就是对消息的一个分类。
生产者将消息发送到特定主题，消费者订阅主题或主题的某些分区进行消费。

### 消息

消息是Kafka通信的基本单位，由一个固定长度的消息头和一个可变长度的消息体构成。
在老版本中，每一条消息称为Message：在由Java重新实现的客户端中，每一条消息称为Record。

### 分区和副本

Kafka将一组消息归纳为一个主题，而每个主题又被分成一个或多个分区（Partition）。
每个分区由一系列有序、不可变的消息组成，是一个有序队列。

每个分区在物理上对应为一个文件夹，分区的命名规则为主题名称后接“-”连接符，之后再接分区编号，分区编号从0开始，编号最大值为分区的总数减l。
每个分区又有一至多个副本（Replica），分区的副本分布在集群的不同代理上，以提高可用性。

分区使得Kafka在井发处理上变得更加容易，理论上来说，分区数越多吞吐量越高，但这要根据集群实际环境及业务场景而定。
同时，分区也是Kafka保证消息被顺序消费以及对消息进行负载均衡的基础。

Kafka只能保证一个分区之内消息的有序性，并不能保证跨分区消息的有序性。
每条消息被追加到相应的分区中，是顺序写磁盘，因此效率非常高，这是Kafka高吞吐率的一个重要保证。

### Leader副本和Follower副本

由于Kafka副本的存在，就需要保证一个分区的多个副本之间数据的一致性，Kafka会选择该分区的一个副本作为Leader副本，
而该分区其他副本即为Follower副本，只有Leader副本才负责处理客户端读／写请求，Follower副本从Leader副本同步数据。

### 偏移量

任何发布到分区的消息会被直接追加到日志文件的尾部，而每条消息在日志文件中的位置都会对应一个按序递增的偏移量。
偏移量是一个分区下严格有序的逻辑值，它并不表示消息在磁盘上的物理位置。

### 生产者

生产者（Producer）负责将消息发送给代理，也就是向Kafka代理发送消息的客户端。

### 消费者和消费组

消费者（Comsumer）以拉取（pull）方式拉取数据，它是消费的客户端。
在Kafka中每一个消费者都属于一个特定消费组（ConsumerGroup），我们可以为每个消费者指定一个消费组，
以`groupId`代表消费组名称，通过`group.id`配置设置。如果不指定消费组，则该消费者属于默认消费组`test-consumer-group`。
同时，每个消费者也有一个全局唯一的id，通过配置项`client.id`指定，如果客户端没有指定消费者的id，
Kafka会自动为该消费者生成一个全局唯一的id，格式为`${groupld }-${hostName}-${timestamp}-${UUID前8位字符}`。

同一个主题的一条消息只能被同一个消费组下某一个消费者消费，但不同消费组的消费者可同时消费该消息。

## 快速启动Kafka服务

### docker-compose

配置`docker-compose.yml`文件：

```yaml
version: '2'

services:
  zookeeper:
    image: 'bitnami/zookeeper:3'
    ports:
      - '2181:2181'
    volumes:
      - 'zookeeper_data:/bitnami'
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
  kafka:
    image: 'bitnami/kafka:2'
    ports:
      - '9092:9092'
    volumes:
      - 'kafka_data:/bitnami'
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper

volumes:
  zookeeper_data:
    driver: local
  kafka_data:
    driver: local
```

执行命令：

```bash
docker-compose up -d
```

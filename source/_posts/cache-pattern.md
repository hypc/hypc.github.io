---
title: 缓存模式
date: 2019-07-15 10:13:29
tags: [cache]
---

常见的模式有分为两大类：Cache-aside以及Cache-as-SoR。

SoR(system-of-record)：记录系统，或者可以叫做数据源，即实际存储原始数据的系统。

Cache：缓存，是SoR的快照数据，Cache的访问速度比SoR要快，放入Cache的目的是提升访问速度，减少回源到SoR的次数。

## Cache-aside

访问记录系统（SoR）的应用程序代码应首先查询缓存，如果缓存包含数据，则直接从缓存返回数据，绕过SoR。
否则，应用程序代码必须从记录系统获取数据，将数据存储在缓存中，然后返回它。写入数据时，必须同时更新缓存以及记录系统。

**读取值的伪代码**

```
v = cache.get(k)
if v == null:
    v = sor.get(k)
    cache.put(k, v)
```

**写入值得伪代码**

```
v = new V()
sor.put(k, v)
cache.put(k, v)
```

<!--more-->

## Cache-as-SoR

在Cache-aside模式下，cache的维护逻辑要业务端自己实现和维护，而Cache-as-SoR则是将cache的逻辑放在存储端，
即`db + cache`对于业务调用方而言是透明的一个整体，业务无须关心实现细节，只需`get/set`即可。
Cache-as-SoR模式常见的有Read Through、Write Through、Write Behind。

### Read Through

Read-Through，业务代码首先调用Cache，如果Cache不命中由Cache回源到SoR，而不是业务代码。

使用Read-Through模式，需要配置一个CacheLoader组件用来回源到SoR加载源数据。

### Write Through

Write-Through，称之为穿透写模式/直写模式，业务代码首先调用Cache写（新增/修改）数据，
然后由Cache负责写缓存和写SoR，而不是业务代码。

使用Write-Through模式需要配置一个CacheWriter组件用来回写SoR。

### Write Behind

Write-Behind，也叫Write-Back，称之为回写模式，不同于Write-Through是同步写SoR和Cache，Write-Behind是异步写。
异步之后可以实现批量写、合并写、延时和限流。

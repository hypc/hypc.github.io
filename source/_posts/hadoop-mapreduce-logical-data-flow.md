---
title: MapReduce逻辑数据流图
date: 2018-12-21 14:00:39
tags: [hadoop, mapreduce, bigdata]
---

![MapReduce逻辑数据流图](/images/hadoop_mapreduce_logical_data_flow.jpg)

1. `input`：将数据转换成`map`的输入；
2. `map`：处理输入的数据，每一行输入处理之后得到一行输出，输出是一个`key-value`格式的数据；
3. `shuffle`：将`map`得到的数据按`key`进行组合，最后得到一个`key-values`的数据；
4. `reduce`：将每一个`key`对应的`values`进行处理，得到结果`key-value2`；
5. `output`：输出结果。

这里需要理解的一个事情是，value的数据类型不仅仅是int，它可以是任意类型，包括数组。

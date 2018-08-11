---
title: 设置maven源
date: 2018-08-11 09:46:11
tags: [maven]
---

打开`setting.xml`，在其中添加以下配置：

```xml
<mirror>
  <id>maven.aliyun.com</id>
  <mirrorOf>central</mirrorOf>
  <name>maven.aliyun.com</name>
  <url>http://maven.aliyun.com/nexus/content/repositories/central</url>
</mirror>
```

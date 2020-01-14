---
title: 使用xmllint处理xml
date: 2019-12-30 10:02:19
tags: [shell]
---

[xmllint][]是一个命令行xml工具。

[xmllint]: http://xmlsoft.org/xmllint.html

## 格式化

使用`--format`参数格式化文件。

假设有文件`books.xml`：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<books><book id="1"><book-name>习惯的力量</book-name><authors><author>查尔斯·都希格</author></authors></book><book id="2"><book-name>从0到1</book-name><authors><author>彼得·蒂尔</author><author>布莱克·马斯特斯</author></authors></book></books>
```

执行命令`xmllint --format books.xml`，得到如下结果：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
  <book id="1">
    <book-name>习惯的力量</book-name>
    <authors>
      <author>查尔斯·都希格</author>
    </authors>
  </book>
  <book id="2">
    <book-name>从0到1</book-name>
    <authors>
      <author>彼得·蒂尔</author>
      <author>布莱克·马斯特斯</author>
    </authors>
  </book>
</books>
```

<!--more-->

## 获取节点的值

使用`--xpath`参数获取节点的值：

```bash
$ xmllint --xpath '//book[@id="1"]/book-name/text()' books.xml
习惯的力量
```

## 获取属性的值

使用`--xpath`参数获取属性的值：

```bash
$ xmllint --xpath '//book/@id' books.xml
id="1" id="2"
```

## 验证xml有效性

使用`--schema`参数验证xml文件的有效性：

```bash
$ xmllite --schema books.xsd books.xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
  <book id="1">
    <book-name>习惯的力量</book-name>
    <authors>
      <author>查尔斯·都希格</author>
    </authors>
  </book>
  <book id="2">
    <book-name>从0到1</book-name>
    <authors>
      <author>彼得·蒂尔</author>
      <author>布莱克·马斯特斯</author>
    </authors>
  </book>
</books>
books.xml validates
```

注意，默认情况下会先输出文件内容，然后给出验证结果。
可以使用`--noout`参数，这样可以只得到验证结果：

```bash
$ xmllite --schema books.xsd --noout books.xml
books.xml validates
```

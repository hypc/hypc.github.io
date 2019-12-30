---
title: zip命令分卷压缩
date: 2018-10-25 19:43:03
tags: [shell]
---

## 创建分卷压缩文件

```bash
zip -s 100m -r file.zip foo/
```

* `-s`: 创建分卷的大小
* `-r`: 循环压缩文件夹下面的内容

切分已有的文件：

```bash
zip existing.zip --out new.zip -s 50m
```

执行完之后会创建下面一系列文件：

```text
new.zip
new.z01
new.z02
new.z03
...
```

## 解压分卷文件

```bash
# 将分卷文件合并成一个单独的文件
zip -s 0 split.zip --out single.zip
# 解压文件
unzip single.zip
```

---
title: python计算MD5值
date: 2018-07-08 11:01:33
tags: [python, md5]
---

## 计算字符串的MD5值

```python
import hashlib

str = ''
md5 = hashlib.md5(str.encode('utf-8')).hexdigest()
print(md5)
```

## 计算文件的MD5值

```python
import hashlib

file = ''
md5file = open(file, 'rb')
md5 = hashlib.md5(md5file.read()).hexdigest()
md5file.close()
print(md5)
```

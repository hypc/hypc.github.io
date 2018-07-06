---
title: Django使用MySQL数据库
date: 2018-07-06 09:51:23
tags: [django, python, mysql]
---

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
        'TEST': {
            'CHARSET': 'utf8',
            'COLLATION': 'utf8_general_ci',
            'NAME': 'test_mydatabase',
        }
    }
}
```

**注意：**

1. 创建数据库：`CREATE DATABASE mydatabase CHARACTER SET utf8;`；
2. 安装mysql依赖：`pip install pymysql`；
3. 配置参数中`CHARSET`、`COLLATION`，若没有这两个参数的话，执行测试用例的时候会报编码错误。

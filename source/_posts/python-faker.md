---
title: Python Faker
date: 2020-07-22 18:51:28
tags: [python]
---

[Faker][]是一个Python包，主要用来创建伪数据，使用[Faker][]包，无需再手动生成或者手写随机数来生成数据，只需要调用[Faker][]提供的方法，即可完成数据的生成。

[Faker]: https://github.com/joke2k/faker

## 安装

```bash
pip install faker
```

## 使用

### 在命令行中使用

安装完之后，可以直接在控制台使用faker命令：

```
usage: faker [-h] [--version] [-v] [-o output] [-l LOCALE] [-r REPEAT] [-s SEP] [--seed SEED] [-i [INCLUDE [INCLUDE ...]]] [fake] [fake argument [fake argument ...]]
```

详细用法可使用`faker --help`查看帮助文档。

<!--more-->

### 在Python代码中使用

在Python代码中使用也非常简单，使用初始化参数`locale`生成器之后，便可开始伪造数据了：

```python
from faker import Faker

fake = Faker(locale='zh_CN')

fake.name()
fake.address()
...
```

**注意**：Faker将数据的生成委托给了`Provider`，并且Faker已经内置了以下`Provider`：

```
address, automotive, bank, barcode, color, company, credit_card,
currency, date_time, file, geo, internet, isbn, job, lorem, misc,
person, phone_number, profile, python, ssn, user_agent
```

## 自定义Provider

```python
from faker import Faker
fake = Faker()

# first, import a similar Provider or use the default one
from faker.providers import BaseProvider

# create new provider class
class MyProvider(BaseProvider):
    def foo(self):
        return 'bar'

# then add new provider to faker instance
fake.add_provider(MyProvider)

# now you can use:
fake.foo()
# 'bar'
```

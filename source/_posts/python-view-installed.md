---
title: 查看Python已安装的模块
date: 2020-06-13 10:06:02
tags: [python, pip]
---

## 使用pip命令查看

在命令行模式下，直接使用`pip freeze`或`pip list`命令查看。

## 在代码中查看

```python
>>> from pip._internal.utils.misc import get_installed_distributions
>>> installed_packages = [f'{_.key}=={_.version}' for _ in get_installed_distributions()]
>>> print('\n'.join(installed_packages))
urllib3==1.25.8
setuptools==41.2.0
requests==2.22.0
pip==20.0.2
...
```

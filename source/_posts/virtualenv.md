---
title: virtualenv
date: 2018-08-05 19:43:10
tags: [python]
---

## 安装

```bash
sudo apt-get install python-virtualenv
```

## 创建虚拟环境

```bash
virtualenv my-env
```

默认情况下，虚拟环境会依赖系统环境中的`site packages`，就是说系统中已经安装好的第三方package也会安装在虚拟环境中，
如果不想依赖这些package，那么可以加上参数`--no-site-packages`建立虚拟环境。

```bash
virtualenv --no-site-packages my-env
```

## 使用虚拟环境

```bash
source my-env/bin/activate
```

## 退出虚拟环境

```bash
deactivate
```

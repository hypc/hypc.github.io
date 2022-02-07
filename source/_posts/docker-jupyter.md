---
title: 使用Docker部署jupyter服务
date: 2021-10-21 21:50:54
tags: [docker, jupyter]
---

Jupyter Notebook 是基于网页的用于交互计算的应用程序。其可被应用于全过程计算：开发、文档编写、运行代码和展示结果。

JupyterLab 是 [Jupyter][] 主打的最新数据科学生产工具，某种意义上，它的出现是为了取代 Jupyter Notebook。
不过不用担心 Jupyter Notebook 会消失，JupyterLab 包含了 Jupyter Notebook 所有功能。

[Jupyter]: https://jupyter.org/

```yaml
version: '3.7'

services:
  jupyter:
    # https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html
    image: jupyter/datascience-notebook
    restart: always
    labels:
      - traefik.http.routers.jupyter.rule=Host(`jupyter.yourdomain.com`)
      - traefik.http.routers.jupyter.entrypoints=websecure
      - traefik.http.routers.jupyter.service=jupyter
      - traefik.http.services.jupyter.loadbalancer.server.port=8888
    environment:
      - JUPYTER_ENABLE_LAB=true
    volumes:
      - data:/home/jovyan

volumes:
  data:
```

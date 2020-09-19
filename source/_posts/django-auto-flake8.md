---
title: Django启动时自动代码检查
date: 2020-08-16 20:49:24
tags: [python, django, flake8]
---

**目标：**执行`python manage.py runserver`时自动进行代码检查。

要达成该目标，可借助Django内置signal：`autoreload_started`。

1. 安装以下packages：

    ```txt
    flake8
    flake8-django
    flake8-assertive
    flake8-return
    flake8-tabs
    flake8-print
    flake8-colors
    flake8-html
    flake8-noqa
    flake8-literal
    flake8-todos
    flake8-raise
    ```

2. 然后修改`manage.py`文件，添加`check_style`函数，内容如下：

    ```python
    @receiver(autoreload_started)
    def check_style(*args, **kwargs):
        try:
            from flake8.main.cli import main as flake8
            flake8([
                "--extend-ignore", "LIT101,DJ08",
                "--max-line-length", "120",
                "--max-doc-length", "150",
                "--tee",
                "--show-source",
                "--max-complexity", "10",
                "--select", "E,F,W,C90",
                "--extend-exclude", "manage.py,*/admin.py,*/migrations/*,*/tests*",
                "--format", ":".join([
                    "${cyan}%(path)s${reset}:${yellow_bold}%(row)d${reset}:${green_bold}%(col)d${reset}",
                    "${red_bold}%(code)s${reset} %(text)s"
                ]),
            ])
        except (ImportError, SystemExit):
            pass
    ```

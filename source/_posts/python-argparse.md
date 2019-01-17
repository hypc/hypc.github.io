---
title: Python命令行参数解析：argparse
date: 2019-01-16 09:42:12
tags: [python]
---

Python标准库中自带了一个[argparse][]模块，这个模块用来解析命令行参数。
当然，python还自带了另外两个库用来实现同样的功能：[getopt][]、[optparse][]。

## 基本用法

一个最简单的例子如下：

```python
# prog.py
import argparse
parser = argparse.ArgumentParser()
parser.parse_args()     # 解析参数
```

执行这段代码，结果如下：

```bash
$ python3 prog.py
$ python3 prog.py --help
usage: prog.py [-h]

optional arguments:
  -h, --help  show this help message and exit
$ python3 prog.py --verbose
usage: prog.py [-h]
prog.py: error: unrecognized arguments: --verbose
$ python3 prog.py foo
usage: prog.py [-h]
prog.py: error: unrecognized arguments: foo
```

<!--more-->

## 添加参数

参数有两种，位置参数和选项参数，区别在于位置参数前没有符号`-`，而选项参数前有符号`-`。

这两种参数的添加都使用[add_argument][]方法：

```python
# prog.py
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("echo", help="echo the string you use here")    # 添加位置参数
parser.add_argument("-v", "--verbosity", help="increase output verbosity")  # 添加选项参数
args = parser.parse_args()  # 解析参数
print(args)
print(args.echo, args.verbosity)
```

执行结果如下：

```bash
$ python prog.py --help
usage: prog.py [-h] [-v VERBOSITY] echo

positional arguments:
  echo                  echo the string you use here

optional arguments:
  -h, --help            show this help message and exit
  -v VERBOSITY, --verbosity VERBOSITY
                        increase output verbosity
$ python prog.py echo_value
Namespace(echo='echo_value', verbosity=None)
echo_value None
$ python prog.py -v verbosity_value echo_value
Namespace(echo='echo_value', verbosity=verbosity_value)
echo_value verbosity_value
```

## add_argument函数

更详细的用法参见：[add_argument][]。

[argparse]: https://docs.python.org/3/library/argparse.html
[getopt]: https://docs.python.org/3/library/getopt.html
[optparse]: https://docs.python.org/3/library/optparse.html
[add_argument]: https://docs.python.org/3/library/argparse.html#the-add-argument-method

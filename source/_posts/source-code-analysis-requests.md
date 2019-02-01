---
title: Requests源码解析
date: 2019-02-01 14:09:40
tags: [python]
---

年终岁尾，也没心思写代码了，后来想想，看看[requests][]源代码吧~

[requests]: https://github.com/requests/requests

--------

## setup.py

首先看这一段代码：

```python
if sys.argv[-1] == 'publish':
    os.system('python setup.py sdist bdist_wheel')
    os.system('twine upload dist/*')
    sys.exit()
```

当执行`python setup.py publish`时会自动编译并上传分发包，
咱们在编写分发包时可以参考这种写法，不过可以优化，我们只上传当前版本的分发包即可。

```python
if sys.argv[-1] == 'publish':
    os.system('python setup.py sdist bdist_wheel')
    os.system('twine upload dist/*{}*'.format(about['__version__']))
    sys.exit()
```

再看这一段代码：

```python
about = {}
with open(os.path.join(here, 'requests', '__version__.py'), 'r', 'utf-8') as f:
    exec(f.read(), about)
```

这段代码有两个写的好的地方：

1. 将一些公共配置信息放到了`requests/__version__.py`文件中；
2. 在执行`setup.py`时不会引入不必要module。

<!--more-->

--------

## requests/__version__.py

```python
__title__ = 'requests'
__description__ = 'Python HTTP for Humans.'
__url__ = 'http://python-requests.org'
__version__ = '2.21.0'
__build__ = 0x022100
__author__ = 'Kenneth Reitz'
__author_email__ = 'me@kennethreitz.org'
__license__ = 'Apache 2.0'
__copyright__ = 'Copyright 2018 Kenneth Reitz'
__cake__ = u'\u2728 \U0001f370 \u2728'
```

正常编写的分发包安装完之后，是无法找到这些信息的，这样写之后，分发包安装完之后也可以看到这部分信息，
另外，在代码中，还可以将这些信息用作逻辑判断。

--------

## requests/__init__.py

```python
from .__version__ import __title__, __description__, __url__, __version__
...
from . import utils
from . import packages
```

在requests中，大量使用相对位置import模块，这也是值得学习的一个地方。

```python
try:
    from urllib3.contrib import pyopenssl
    pyopenssl.inject_into_urllib3()

    # Check cryptography version
    from cryptography import __version__ as cryptography_version
    _check_cryptography(cryptography_version)
except ImportError:
    pass
```

这是一段检测依赖包版本的代码，我们在写代码的时候会依赖各种包，
如果版本不一致可能会导致各种未知的情况，可以按照这段代码的写法检测依赖包的版本问题。

--------

## requests/session.py

```python
class Session(SessionRedirectMixin):
    ...
    def __enter__(self):
        return self

    def __exit__(self, *args):
        self.close()

    def close(self):
        """Closes all adapters and as such the session"""
        for v in self.adapters.values():
            v.close()
    ...
```

类Session是一个标准上下文管理器的写法，它可以直接使用with语句块进行代码编写：

```python
with requests.Session() as s:
    s.get('https://httpbin.org/get')
```

这也是一个值得我们学习的地方，当我们编写一段涉及资源申请、使用、销毁的代码时，可以按这种方式进行封装。

--------

## requests/api.py

```python
def request(method, url, **kwargs):
    with sessions.Session() as session:
        return session.request(method=method, url=url, **kwargs)

def get(url, params=None, **kwargs):
    kwargs.setdefault('allow_redirects', True)
    return request('get', url, params=params, **kwargs)
```

这也是一个值得学习的地方，当我们想读一个配置文件可以采用这种写法：

```python
def read_configs(config_path):
    with open(config_path, 'r') as f:
        return f.read()

configs = read_configs('/path/to/config')
```

--------

## requests/exceptions.py

这个文件中都是一些自定义异常，我认为这里有两个需要学习的地方：

1. 在一个文件中定义异常，方便统一管理；
2. 先定义一个基类异常，然后在这个基类的基础上定义各种业务的异常。

--------

## requests/compat.py

```python
# Syntax sugar.
_ver = sys.version_info
#: Python 2.x?
is_py2 = (_ver[0] == 2)
#: Python 3.x?
is_py3 = (_ver[0] == 3)

if is_py2:
    ...
elif is_py3:
    ...
```

如果你的代码需要兼容Python2和Python3的话，这是一个很好的例子。

--------

## requests/utils.py

```python
if sys.platform == 'win32':
    ...
```

当你的代码需要考虑平台兼容性时，可以这么做，`sys.platform`可能得值有：
`linux`、`win32`、`cygwin`、`darwin`等。

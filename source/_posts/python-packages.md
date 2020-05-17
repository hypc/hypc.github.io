---
title: Python分发包
date: 2018-08-05 19:30:54
tags: [python, pip]
---

## 创建Python项目

创建一个简单Python项目，在项目中添加文件`setup.py`：

```python
import os
from setuptools import setup, find_packages
from setuptools.command.install import install

# 1.2.0.dev1  # Development release
# 1.2.0a1     # Alpha Release
# 1.2.0b1     # Beta Release
# 1.2.0rc1    # Release Candidate
# 1.2.0       # Final Release
# 1.2.0.post1 # Post Release
# 15.10       # Date based release
# 23          # Serial release
VERSION = '0.0.1a1'
PATH = os.path.dirname(os.path.abspath(__file__))

# When the project is installed by pip, this is the specification that is used to install its dependencies.
install_requires = []


def read(fname):
    return open(os.path.join(PATH, fname)).read()


class PostInstallCommand(install):
    def run(self):
        pass


setup(
    # This is the name of your project, determining how your project is listed on PyPI.
    name='sample',
    version=VERSION,
    # Give a short and long description for your project. These values will be displayed on PyPI if you publish your project.
    description='',
    long_description=read('README.md'),
    # Give a homepage URL for your project.
    url='',
    # Provide details about the author.
    author='',
    author_email='',
    license='MIT',
    # List keywords that describe your project.
    keywords='',
    # List additional relevant URLs about your project.
    project_urls=[],
    packages=find_packages(exclude=['docs', 'tests*', 'example']),
    install_requires=install_requires,
    # If your project only runs on certain Python versions, setting the `python_requires` argument.
    python_requires='>=3',
    classifiers=[
        # How mature is this project? Common values are
        #   3 - Alpha
        #   4 - Beta
        #   5 - Production/Stable
        'Development Status :: 3 - Alpha',

        # Indicate who your project is intended for
        'Intended Audience :: Developers',
        'Topic :: Software Development :: Build Tools',

        # Pick your license as you wish (should match "license" above)
        'License :: OSI Approved :: MIT License',

        # Specify the Python versions you support here. In particular, ensure
        # that you indicate whether you support Python 2, Python 3 or both.
        'Programming Language :: Python :: 3',
        'Programming Language :: Python :: 3.2',
        'Programming Language :: Python :: 3.3',
        'Programming Language :: Python :: 3.4',
        'Programming Language :: Python :: 3.5',
    ],
    entry_points={
        # You can then let the toolchain handle the work of turning these interfaces into actual scripts.
        'console_scripts': [
            'sample=sample:main',
        ],
    },
    cmd_class={
        'install': PostInstallCommand,
    }
)
```

其中`setup()`的具体参数详见：[setup-args][]和[setuptools][]。

<!--more-->

## 创建`~/.pypirc`文件

```
[distutils]
index-servers =
    pypi
    myrepo

[pypi]
username: username
password: password

[myrepo]
repository: https://pypi.myrepo.com/
username: username
password: password
```

`.pypirc`文件的具体配置参数详见：[pypirc-file][]。

## 打包并上传

```bash
pip install -U setuptools twine wheel
python setup.py sdist bdist_wheel
twine upload dist/*     # 将分发包上传到默认服务器上，默认是`pypi`
twine upload -r myrepo dist/*   # 将分发包上传到指定服务器上
```

其中`twine`的具体参数详见：[twine][]。


[setup-args]: https://packaging.python.org/tutorials/distributing-packages/#setup-args
[setuptools]: https://setuptools.readthedocs.io/en/latest/setuptools.html
[pypirc-file]: https://docs.python.org/3.5/distutils/packageindex.html#the-pypirc-file
[twine]: https://pypi.python.org/pypi/twine

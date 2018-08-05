---
title: pyenv
date: 2018-08-05 19:39:48
tags: [python]
---

## 安装pyenv

执行：

```bash
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

然后在`~/.bashrc`或`~/.bash_profile`或`~/.profile`文件中添加以下文本：

```bash
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## 安装python

执行下面命令：

```bash
$ pyenv install 版本号
```

<!--more-->

## pyenv加速

1. 先下载需要安装的版本到`~/.pyenv/cache/`目录下；
2. 然后执行`pyenv install 版本号`安装对应的版本。

## 问题

### patch: command not found

**详细情况如下**

```bash
$ pyenv install 3.5.2
Installing Python-3.5.2...
/root/.pyenv/plugins/python-build/bin/python-build: line 1477: patch: command not found

BUILD FAILED (CentOS Linux 7 using python-build 20160602)

Inspect or clean up the working tree at /tmp/python-build.20161126141738.1062
Results logged to /tmp/python-build.20161126141738.1062.log

Last 10 log lines:
/tmp/python-build.20161126141738.1062 ~
/tmp/python-build.20161126141738.1062/Python-3.5.2 /tmp/python-build.20161126141738.1062 ~
```

**解决办法**

```bash
$ apt-get install -y patch  # ubuntu
$ yum install -y patch      # centos/redhat
```

### pip 8.1.1 requires SSL/TLS

**详细情况如下**

```bash
$ pyenv install 3.5.2
Installing Python-3.5.2...
patching file Lib/venv/scripts/posix/activate.fish
WARNING: The Python bz2 extension was not compiled. Missing the bzip2 lib?
WARNING: The Python readline extension was not compiled. Missing the GNU readline lib?
ERROR: The Python ssl extension was not compiled. Missing the OpenSSL lib?

Please consult to the Wiki page to fix the problem.
https://github.com/yyuu/pyenv/wiki/Common-build-problems


BUILD FAILED (CentOS Linux 7 using python-build 20160602)

Inspect or clean up the working tree at /tmp/python-build.20161126142627.2850
Results logged to /tmp/python-build.20161126142627.2850.log

Last 10 log lines:
(cd /root/.pyenv/versions/3.5.2/share/man/man1; ln -s python3.5.1 python3.1)
if test "xupgrade" != "xno"  ; then \
    case upgrade in \
        upgrade) ensurepip="--upgrade" ;; \
        install|*) ensurepip="" ;; \
    esac; \
     ./python -E -m ensurepip \
        $ensurepip --root=/ ; \
fi
Ignoring ensurepip failure: pip 8.1.1 requires SSL/TLS
```

**解决办法**

```bash
$ yum install -y openssl-devel  # centos/redhat
```

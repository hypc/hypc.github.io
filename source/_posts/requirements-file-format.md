---
title: Requirements File Format
date: 2020-07-14 20:28:24
tags: [python, pip]
---

`Requirements`文件是包含需要使用`pip install`安装的项目列表文件。

`Requirements`文件的每一行都是一条要安装的内容，就像`pip install`的参数一样，支持以下形式：

```
[[--option]...]
<requirement specifier> [; markers] [[--option]...]
<archive url/path>
[-e] <local project path>
[-e] <vcs project url>
```

可以使用`#`来表示注释，行尾以`\`表示折行。

<!--more-->

## Options

* [-i, --index-url](https://pip.pypa.io/en/stable/reference/pip_install/#install-index-url)
* [--extra-index-url](https://pip.pypa.io/en/stable/reference/pip_install/#install-extra-index-url)
* [--no-index](https://pip.pypa.io/en/stable/reference/pip_install/#install-no-index)
* [-c, --constraint](https://pip.pypa.io/en/stable/reference/pip_install/#install-constraint)
* [-r, --requirement](https://pip.pypa.io/en/stable/reference/pip_install/#install-requirement)
* [-e, --editable](https://pip.pypa.io/en/stable/reference/pip_install/#install-editable)
* [-f, --find-links](https://pip.pypa.io/en/stable/reference/pip_install/#install-find-links)
* [--no-binary](https://pip.pypa.io/en/stable/reference/pip_install/#install-no-binary)
* [--only-binary](https://pip.pypa.io/en/stable/reference/pip_install/#install-only-binary)
* [--require-hashes](https://pip.pypa.io/en/stable/reference/pip_install/#install-require-hashes)
* [--pre](https://pip.pypa.io/en/stable/reference/pip_install/#install-pre)
* [--trusted-host](https://pip.pypa.io/en/stable/reference/pip/#trusted-host)

## Requirement Specifiers

`requirement specifier`由项目名称和可选的[版本说明](https://www.python.org/dev/peps/pep-0440/#version-specifiers)组成。如：

```requirements
SomeProject
SomeProject == 1.3
SomeProject >=1.2,<2.0
SomeProject[foo, bar]
SomeProject~=1.4.2
```

从6.0开始，pip支持[environment markers](https://www.python.org/dev/peps/pep-0508/#environment-markers)。如：

```requirements
SomeProject ==5.4 ; python_version < '2.7'
SomeProject; sys_platform == 'win32'
```

从19.1开始，pip支持[direct references](https://www.python.org/dev/peps/pep-0440/#direct-references)，如：

```requirements
pip @ file:///localbuilds/pip-1.3.1.zip
pip @ https://github.com/pypa/pip/archive/1.3.1.zip#sha1=da9234ee9982d4bbb3c72346a6de940a148ea686
pip @ git+https://github.com/pypa/pip.git@7921be1537eac1e97bc40179a57f0349c2aee67d
pip @ git+https://github.com/pypa/pip.git@1.3.1#7921be1537eac1e97bc40179a57f0349c2aee67d
```

从7.0版开始，pip支持通过`requirements file`控制为`setup.py`提供的命令行选项。

`--global-option`和`--install-option`选项用于将选项传递给`setup.py`。例如：

```requirements
FooProject >= 1.2 --global-option="--no-user-cfg" \
                  --install-option="--prefix='/usr/local'" \
                  --install-option="--no-compile"
```

相当于运行了：

```bash
python setup.py --no-user-cfg install --prefix='/usr/local' --no-compile
```

## Archives

archive url/path.

## Local Project Path

local project path.

## Vcs Project Url

pip支持从`Git`，`Mercurial`，`Subversion`和`Bazaar`安装，并使用URL前缀`git+`，`hg+`，`svn+`和`bzr+`检测VCS的类型。

**Git**

```requirements
[-e] git+http://git.example.com/MyProject#egg=MyProject
[-e] git+https://git.example.com/MyProject#egg=MyProject
[-e] git+ssh://git.example.com/MyProject#egg=MyProject
[-e] git+file:///home/user/projects/MyProject#egg=MyProject
```

**Mercurial**

```requirements
[-e] hg+http://hg.myproject.org/MyProject#egg=MyProject
[-e] hg+https://hg.myproject.org/MyProject#egg=MyProject
[-e] hg+ssh://hg.myproject.org/MyProject#egg=MyProject
[-e] hg+file:///home/user/projects/MyProject#egg=MyProject
```

**Subversion**

```requirements
[-e] svn+https://svn.example.com/MyProject#egg=MyProject
[-e] svn+ssh://svn.example.com/MyProject#egg=MyProject
[-e] svn+ssh://user@svn.example.com/MyProject#egg=MyProject
```

**Bazaar**

```requirements
[-e] bzr+http://bzr.example.com/MyProject/trunk#egg=MyProject
[-e] bzr+sftp://user@example.com/MyProject/trunk#egg=MyProject
[-e] bzr+ssh://user@example.com/MyProject/trunk#egg=MyProject
[-e] bzr+ftp://user@example.com/MyProject/trunk#egg=MyProject
[-e] bzr+lp:MyProject#egg=MyProject
```

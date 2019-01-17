---
title: RobotFramework问题集
date: 2018-12-28 16:34:31
tags: [python, robotframework, testing]
---

> Importing test library 'Selenium2Library' failed: ImportError: No module named 'Selenium2Library'

安装`seleniumlibrary`时需要注意有两个包，`robotframework-seleniumlibrary`和`robotframework-selenium2library`。

```bash
pip install robotframework robotframework-seleniumlibrary
# or
pip install robotframework robotframework-selenium2library
```

----

> WebDriverException: Message: 'geckodriver' executable needs to be in PATH.

找不到geckodriver，使用`brew install geckodriver`命令安装。<!--more-->

----

> SessionNotCreatedException: Message: Unable to find a matching set of capabilities

驱动firefox浏览器失败，可能原因是本地没有firefox浏览器，使用Chrome替代。

----

> WebDriverException: Message: 'chromedriver' executable needs to be in PATH. Please see https://sites.google.com/a/chromium.org/chromedriver/home

Chrome浏览器驱动失败，使用`brew cask install chromedriver`安装驱动。

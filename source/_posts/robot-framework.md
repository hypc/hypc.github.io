---
title: Robot Framework
date: 2020-06-26 17:05:33
tags: [robotframework, testing]
---

## 介绍

[Robot Framework][]是基于Python的可扩展的关键字驱动的自动化框架，用于验收测试，验收测试驱动开发（ATDD），行为驱动开发（BDD）和机器人流程自动化（RPA）。
它可用于分布式，异构环境，在这些环境中，自动化需要使用不同的技术和界面。

该框架周围有一个丰富的生态系统，由作为单独项目开发的各种通用库和工具组成。

### 为什么使用Robot Framework

* 启用易于使用的表格语法，以统一的方式创建测试用例。
* 提供从现有关键字创建可重用的高级关键字的功能。
* 提供易于阅读的结果报告和HTML格式的日志。
* 是平台和应用程序独立的。
* 提供一个简单的库API，用于创建自定义的测试库，该库可以用Python或Java本地实现。
* 提供命令行界面和基于XML的输出文件，以集成到现有的构建基础结构（连续集成系统）中。<!--more-->
* 提供对Selenium的支持，以进行Web测试，Java GUI测试，正在运行的进程，Telnet，SSH等。
* 支持创建数据驱动的测试用例。
* 具有对变量的内置支持，特别适用于在不同环境中进行测试。
* 提供标记以分类和选择要执行的测试用例。
* 支持与源代码控制的轻松集成：测试套件只是可以用生产代码进行版本控制的文件和目录。
* 提供测试用例和测试套件级别的设置和拆卸。
* 模块化架构甚至支持为具有多个不同接口的应用程序创建测试。

### 整体架构

[Robot Framework][]是一个通用的，独立于应用程序和技术的框架。 它具有高度模块化的架构，如下图所示：

![](/images/robot-framework-1.png)

Test Data采用简单、易于编辑的表格格式。当启动[Robot Framework][]时，它将处理数据，执行测试用例并生成日志和报告。
核心框架对被测目标一无所知，与目标的交互将由库处理。库可以直接使用应用程序界面，也可以使用较低级别的测试工具作为驱动程序。

### 截图

以下屏幕截图显示了测试数据以及创建的报告和日志的示例：

![](/images/robot-framework-2.png "测试用例")
![](/images/robot-framework-3.png "报告和日志")

## 安装及执行

### 安装或升级

```bash
pip install robotframework              # 安装
pip install --upgrade robotframework    # 升级
```

### 执行测试

使用`robot`命令执行robot脚本，使用rebot对结果进行后续处理：

```bash
robot tests.robot
rebot output.xml
```

## 示例

* [Quick Start Guide][]: 介绍了Robot Framework的主要功能，并充当可执行演示。
* [Robot Framework demo][]: 简单的示例测试用例。演示还创建自定义测试库。
* [Web testing demo][]: 演示如何创建测试和更高级别的关键字。被测系统是一个使用[SeleniumLibrary][]测试的简单网页。
* [SwingLibrary demo][]: 演示使用[SwingLibrary][]测试Java GUI应用程序。
* [ATDD with Robot Framework][]: 演示在遵循验收测试驱动开发（ATDD）流程时如何使用Robot Framework。


[Robot Framework]: https://robotframework.org/
[Quick Start Guide]: https://github.com/robotframework/QuickStartGuide/blob/master/QuickStart.rst
[Robot Framework demo]: https://github.com/robotframework/RobotDemo
[Web testing demo]: https://github.com/robotframework/WebDemo
[SwingLibrary demo]: https://github.com/robotframework/SwingLibrary/wiki/SwingLibrary-Demo
[ATDD with Robot Framework]: https://code.google.com/p/atdd-with-robot-framework
[SeleniumLibrary]: https://github.com/robotframework/SeleniumLibrary
[SwingLibrary]: https://github.com/robotframework/SwingLibrary

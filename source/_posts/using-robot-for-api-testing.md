---
title: 使用Robot Framework做API测试
date: 2020-06-28 19:24:00
tags: [robotframework, testing]
---

## 什么是API

API（Application Programming Interface，应用程序接口）是一些预先定义的函数，或指软件系统不同组成部分衔接的约定。

![](/images/using-robot-for-api-testing.png)

## 什么是API测试

API测试与UI测试不同，UI测试主要关注表现层，而API测试主要关注业务逻辑层。
着重强调的是逻辑调用关系，而不是UI操作或用户感官。

<!--more-->

## Get Starting

### Library

有几个常用的库需要了解：

* [Robot标准库][]: Robot已经内置的许多库，下面是一些常用的库：
  - BuiltIn
  - Collections
  - DateTime
  - String
* [RequestsLibrary][]: 是一个用来发起HTTP请求的扩展库。
* [FakerLibrary][]: 一个用来伪造数据的扩展库。
* [JSONSchemaLibrary][]: 一个使用jsonschema来测试json数据的扩展库。

[Robot标准库]: http://robotframework.org/robotframework/
[RequestsLibrary]: http://marketsquare.github.io/robotframework-requests/
[FakerLibrary]: https://guykisel.github.io/robotframework-faker/
[JSONSchemaLibrary]: https://github.com/hypc/robot-jsonschemalibrary

### 测试项目的组织结构

如果只是进行针对少量API接口的测试，怎么写都没有什么关系。
但如果涉及大量（数十甚至数百）接口，那么项目的组织结构就需要好好构思一下了。
否则的话，测试用例的可维护性、可读性将会非常的差。

![](/images/using-robot-for-api-testing-1.png)

### 编写测试用例代码

#### `README.rst`文件

当前项目的说明性文档。

#### `requirements.txt`文件

存放运行本项目的测试用例用到的库。内容如下：

```
robotframework
robotframework-requests
robotframework-faker
robot-jsonschemalibrary
```

#### `tests/resources.robot`文件

这个文件存放一些全局的变量、参数，以及一些通用的keywords。代码如下：

```rst
*** Settings ***
Documentation           This is a resource file.
Library                 RequestsLibrary


*** Variables ***
${GITHUB_API_DOMAIN}    https://api.github.com
```

#### `tests/*`目录

这些目录下存放的都是测试用例文件，目录的创建规则如下：

* 以API接口的URL进行创建目录，动态部分以`$`开头
* 以请求方法创建测试用例文件

例如：

* `GET /gists` --> `/gists/get.robot`
* `GET /gists/{gist_id}` --> `/gists/$gist_id/get.robot`
* `GET /repos/{owner}/{repo}/issues` --> `/repos/$owner/$repo/issues/get.robot`

下面是`/repos/$owner/$repo/issues/get.robot`的代码：

```rst
*** Settings ***
Resource                ${EXECDIR}/tests/resources.robot
Library                 String
Suite Setup             Create Session    github    ${GITHUB_API_DOMAIN}
Suite Teardown          Delete All Sessions


*** Variables ***
${API_URI}              /repos/{owner}/{repo}/issues


*** Test Cases ***
GET Repo Issues Success
    ${url}=             Format String    ${API_URI}    owner=google    repo=jax
    ${resp}=            Get Request    github    ${url}
    Request Should Be Successful    ${resp}
    Status Should Be    200    ${resp}

GET Repo Issues Failure
    ${url}=             Format String    ${API_URI}    owner=not_exists_user    repo=none
    ${resp}=            Get Request    github    ${url}
    Status Should Be    404    ${resp}
```

### 执行测试用例

使用下面命令执行测试用例：

```bash
robot -d reports tests/
```

测试报告如下：

![](/images/using-robot-for-api-testing-2.png)

![](/images/using-robot-for-api-testing-3.png)

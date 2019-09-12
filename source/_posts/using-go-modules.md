---
title: 使用Go Modules
date: 2019-09-12 09:37:57
tags: [golang]
---

`go modules`是golang1.11新加的特性。将环境变量`GO111MODULE`设置为`on`来开启`modules`功能。

## `go mod`命令

golang提供了`go mod`命令来管理包。

```shell
$ go help mod
Go mod provides access to operations on modules.

Note that support for modules is built into all the go commands,
not just 'go mod'. For example, day-to-day adding, removing, upgrading,
and downgrading of dependencies should be done using 'go get'.
See 'go help modules' for an overview of module functionality.

Usage:

    go mod <command> [arguments]

The commands are:

    download    download modules to local cache
    edit        edit go.mod from tools or scripts
    graph       print module requirement graph
    init        initialize new module in current directory
    tidy        add missing and remove unused modules
    vendor      make vendored copy of dependencies
    verify      verify dependencies have expected content
    why         explain why packages or modules are needed

Use "go help mod <command>" for more information about a command.
```

<!--more-->

## 在项目中使用`go modules`

### 创建一个新项目

创建一个新项目，并使用`go mod init`初始化：

```shell
$ mkdir hello
$ cd hello
$ go mod init
```

初始化之后会创建`go.mod`文件，`go.mod`提供了4个指令：`module`、`require`、`replace`、`exclude`。

* `module`: 指定包的名字
* `require`: 指定依赖项模块
* `replace`: 指定可以替换的依赖项模块
* `exclude`: 指定可以忽略的依赖项模块

### 添加依赖

新建一个`main.go`文件，内容如下：

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.GET("/hello", func(c *gin.Context) {
		c.String(http.StatusOK, "Hello World!")
	})
	r.Run(":8080")
}
```

直接执行`go run main.go`，会发现`go mod`会自动检查并加载依赖。

### 使用`go get`命令

除了直接执行程序自动加载依赖以外，还可以使用`go get`命令添加或升级依赖。

* `go get -u`将会升级到最新的次要版本或者修订版本
* `go get package@version`，将会获取指定version的package

执行`go get`命令会自动更新`go.mod`文件。

## `GOPROXY`设置

当我们下载依赖包时，由于一些原因，导致下载失败，这时候我们可以通过设置`GOPROXY`来解决这个问题。

```shell
export GOPROXY=https://mirrors.aliyun.com/goproxy/
```

我们也可以通过[goproxy][]来构建自己的私有服务。

[goproxy]: https://github.com/goproxyio/goproxy

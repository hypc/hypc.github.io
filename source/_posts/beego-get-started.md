---
title: Beego快速入门
date: 2019-08-16 14:21:50
tags: [golang, beego]
---

[Beego][]是一个快速开发Go应用的HTTP框架，他可以用来快速开发API、Web及后端服务等各种应用，是一个RESTful的框架，
主要设计灵感来源于tornado、sinatra和flask这三个框架，但是结合了Go本身的一些特性（interface、struct嵌入等）而设计的一个框架。

## beego执行逻辑

[Beego][]是一个典型的MVC架构，它的执行逻辑如下图所示：

![](/images/beego-get-started-1.png)

<!--more-->

[Beego]: https://beego.me/

## beego项目结构

beego项目一般结构为：

```
proj
|-- conf
|   |-- app.conf
|-- controllers
|   |-- admin
|   |-- default.go
|-- main.go
|-- models
|   |-- models.go
|-- static
|   |-- css
|   |-- ico
|   |-- img
|   |-- js
|-- views
    |-- admin
    |-- index.tpl
```

从上面的目录结构我们可以看出来`M`（models）、`V`（views）和`C`（controllers）的结构，`main.go`是入口文件。

## Hello World

安装或升级beego：

```bash
$ go get -u github.com/astaxie/beego
```

`hello.go`代码如下：

```go
package main

import (
    "github.com/astaxie/beego"
)

type MainController struct {
    beego.Controller
}

func (this *MainController) Get() {
    this.Ctx.WriteString("hello world")
}

func main() {
    beego.Router("/", &MainController{})
    beego.Run()
}
```

编译并且运行服务：

```bash
$ go build -o hello hello.go
$ ./hello
```

打开浏览器浏览`http://localhost:8080`，将看到返回`hello world`。

## 使用bee命令快速创建应用

安装或升级bee：

```bash
$ go get -u github.com/beego/bee
```

快速创建应用并应用：

```bash
$ cd $GOPATH/src
$ bee new hello
$ cd hello
$ bee run
```

打开浏览器浏览`http://localhost:8080`。

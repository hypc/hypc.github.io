---
title: GoPath & GoModule
date: 2021-09-13 16:45:51
tags: [golang]
---

`GoPath` 是 `Golang` 的工作空间，所有的 go 文件都必须放在 `GoPath` 目录下，才能编译运行。
这样做有以下明显的缺点：

1.  第三方依赖的包和我们自己的 `Golang` 包混在一起，会给我们的项目文件管理带来一定的麻烦。
2.  不同的 `GoPath` 都需要下载依赖，那么磁盘中重复的依赖就会非常多，会占用我们大量的磁盘空间。

`GoModule` 是 `go1.11` 初步引入，`go1.12` 正式引入的概念，它用来管理项目文件。
但是在 `go1.11`、`go1.12` 中默认是关闭的，可以设置 `GO111MODULE=on` 来开启 `GoModule`。
从 `go1.13` 开始，`Golang` 默认开启 `GoModule`，它会根据项目结构中是否包含 `go.mod` 文件来自动判断。

由此，引出一个疑问：**有了 `GoModule` 之后，`GoPath` 是否无用了？**

其实不然，在有了 `GoModule` 之后，`GoPath`、`GoModule` 可以分别负责不同的职责：

* **使用 `GoPath` 管理 `Golang` 依赖**
* **使用 `GoModule` 管理项目文件**

> 在使用 `GoModule` 时，需要先执行命令 `go mod init 模块名` 进行初始化，例如：
> `go mod init github.com/user/example`

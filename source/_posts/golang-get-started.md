---
title: Go快速入门
date: 2019-08-16 08:57:35
tags: [golang]
---

2009年11月，Google发布了Go语言，在世界范围内引发了轰动。
2015年和2016年中国区的Go语言大会分别在上海和北京召开，来自Go语言团队的开发人员均作了相关的报告。
纵观这几年来的发展趋势，Go语言已经成为云计算、云存储时代最重要的基础编程语言。

## 安装

[下载][go downloads]相应的压缩包，并解压：

```shell
$ tar -C /usr/local -xvf go$VERSION.$OS-$ARCH.tar.gz
```

设置环境变量：

```shell
export PATH=$PATH:/usr/local/go/bin
```

[go downloads]: https://golang.org/dl/

<!--more-->

## Hello World

创建你的工作目录`$HOME/go`（可以使用其他目录），并设置环境变量：

```shell
export GOPATH=$HOME/go
export PATH=$PATH:$(go env GOPATH)/bin
```

然后在其中创建目录`src/hello`，并且创建文件`hello.go`：

```go
package main

import "fmt"

func main() {
	fmt.Printf("hello, world\n")
}
```

编译并且执行：

```shell
$ cd $GOPATH/src/hello
$ go build
$ ./hello
hello, world
```

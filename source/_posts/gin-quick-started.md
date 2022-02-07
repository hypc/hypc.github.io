---
title: gin快速入门
date: 2021-11-11 15:23:48
tags: [golang, gin]
---

[Gin][] 是使用 Go 实现的 HTTP Web 框架。

[Gin]: https://gin-gonic.com/

安装：

```bash
$ go get -u github.com/gin-gonic/gin
```

编写 `main.go` 文件：

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080
}
```

运行：

```bash
# run main.go and vist http://0.0.0.0:8080/ping on browser
$ go run main.go
```

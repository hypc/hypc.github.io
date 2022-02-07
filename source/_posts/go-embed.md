---
title: 使用 Embed 嵌入静态资源
date: 2022-01-27 14:19:52
tags: [golang]
---

go1.16 提供了一个新功能 [embed][]，使用 [embed][] 可以在编译的时候将静态文件资源嵌入到程序中。

[embed]: https://pkg.go.dev/embed

在代码中使用 `//go:embed` 指令可以将文件资源映射为 `string`、`[]byte`、`embed.FS` 类型。

将一个文件嵌入为 `string`：

```go
import _ "embed"

//go:embed hello.txt
var s string
print(s)
```

将一个文件嵌入为 `[]byte`：

```go
import _ "embed"

//go:embed hello.txt
var b []byte
print(string(b))
```

将一个文件嵌入为 `embed.FS`：

```go
import "embed"

//go:embed hello.txt
var f embed.FS
data, _ := f.ReadFile("hello.txt")
print(string(data))
```

同时嵌入多个静态资源：

```go
import "embed"

//go:embed assets/* templates/*
var f embed.FS
```

---
title: 使用 chan + goroutine 实现生成器
date: 2022-01-06 09:17:21
tags: [golang]
---

在其他语言中可以直接使用 `yield` 实现生成器的用法，go 中并没有直接实现生成器的用法。
但是我们可以使用 chan + goroutine 的方式实现生成器：

```go
package main

import "fmt"

func Generator() chan int {
    ch := make(chan int)
    go func() {
        defer close(ch)
        for i := 0; i < 100; i++ {
            ch <- i
        }
    }()
    return ch
}

func main() {
    for i := range Generator() {
        fmt.Println(i)
    }
}
```

**重要**

在 chan 使用完之后记得关闭，否则读取完最后一条数据之后，进程将处于堵塞状态。

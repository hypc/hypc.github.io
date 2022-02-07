---
title: http 请求及响应解析
date: 2022-01-10 09:55:15
tags: [golang]
---

包 [net/http][] 提供了 HTTP 客户端及服务器的实现。

[net/http]: https://pkg.go.dev/net/http

## Get

```go
import (
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	resp, err := http.Get("https://httpbin.org/get")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer resp.Body.Close()

	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err)
		return
	}
	if resp.StatusCode > 299 {
		fmt.Printf("Response failed with status code: %d and\nbody: %s\n", resp.StatusCode, body)
		return
	}
	fmt.Println(string(body))
}
```

<!--more-->

## PostForm

```go
import (
	"fmt"
	"io/ioutil"
	"net/http"
	"net/url"
)

func main() {
	params := url.Values{}
	params.Set("param1", "value1")
	params.Set("param2", "value2")

	resp, err := http.PostForm("https://httpbin.org/post", params)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer resp.Body.Close()

	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err)
		return
	}
	if resp.StatusCode > 299 {
		fmt.Printf("Response failed with status code: %d and\nbody: %s\n", resp.StatusCode, body)
		return
	}
	fmt.Println(string(body))
}
```

## Complex Post

```go
import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
)

func main() {
	params, _ := json.Marshal(map[string]interface{}{
		"param1": "value1",
		"param2": "value2",
	})

	client := &http.Client{}
	req, err := http.NewRequest("POST", "https://httpbin.org/post", bytes.NewReader(params))
	if err != nil {
		fmt.Println(err)
		return
	}
	req.Header.Set("Content-Type", "application/json")

	resp, err := client.Do(req)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer resp.Body.Close()

	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		fmt.Println(err)
		return
	}
	if resp.StatusCode > 299 {
		fmt.Printf("Response failed with status code: %d and\nbody: %s\n", resp.StatusCode, body)
		return
	}
	fmt.Println(string(body))
}
```

---
title: json 序列化及反序列化
date: 2022-01-10 14:32:15
tags: [golang]
---

包 [encoding/json][] 提供了有关 json 的操作：

* `json.Marshal`: 序列化 json
* `json.Unmarshal`: 反序列化 json 字符串

[encoding/json]: https://pkg.go.dev/encoding/json

## 基础类型序列化及反序列化

直接使用 `json.Marshal` 进行序列化，使用 `json.Unmarshal` 反序列化 json 字符串。
在进行反序列化时需要注意接受数据的变量类型。

## map 序列化及反序列化

```go
import (
	"encoding/json"
	"fmt"
)

func main() {
	article := map[string]interface{}{
		"article_id":    6931630572720619534,
		"user_id":       465848661970824,
		"category_id":   6809637769959178254,
		"tag_ids":       []int{6809640445233070094},
		"link_url":      "",
		"cover_image":   "",
		"title":         "spring中那些让你爱不释手的代码技巧",
		"brief_content": "最近越来越多的读者认可我的文章，还是件挺让人高兴的事情。有些读者私信我说希望后面多分享spring方面的文章，这样能够在实际工作中派上用场。正好我对spring源码有过一定的研究，并结合我这几年实际的工作经验，把spring中我认为不错的知识点总结一下，希望对您有所帮助。 实现…",
		"is_english":    0,
		"is_original":   1,
		"content":       "",
		"ctime":         1613896165,
		"mtime":         1625579546,
		"rtime":         1613994419,
		"view_count":    10374,
		"collect_count": 533,
		"digg_count":    339,
		"comment_count": 22,
		"status":        2,
	}
	str, _ := json.Marshal(article)
	fmt.Println(string(str))
	var article2 map[string]interface{}
	_ = json.Unmarshal(str, &article2)
	fmt.Println(article2)
}
```

<!--more-->

## struct 序列化及反序列化

```go
import (
	"encoding/json"
	"fmt"
)

type Article struct {
	ArticleId    int    `json:"article_id"`
	UserId       int    `json:"user_id"`
	CategoryId   int    `json:"category_id"`
	TagIds       []int  `json:"tag_ids"`
	LinkUrl      string `json:"link_url"`
	CoverImage   string `json:"cover_image"`
	Title        string `json:"title"`
	BriefContent string `json:"brief_content"`
	IsEnglish    int    `json:"is_english"`
	IsOriginal   int    `json:"is_original"`
	Content      string `json:"content"`
	Ctime        int    `json:"ctime"`
	Mtime        int    `json:"mtime"`
	Rtime        int    `json:"rtime"`
	ViewCount    int    `json:"view_count"`
	CollectCount int    `json:"collect_count"`
	DiggCount    int    `json:"digg_count"`
	CommentCount int    `json:"comment_count"`
	Status       int    `json:"status"`
}

func main() {
	article := &Article{
		ArticleId:    6931630572720619534,
		UserId:       465848661970824,
		CategoryId:   6809637769959178254,
		TagIds:       []int{6809640445233070094},
		LinkUrl:      "",
		CoverImage:   "",
		Title:        "spring中那些让你爱不释手的代码技巧",
		BriefContent: "最近越来越多的读者认可我的文章，还是件挺让人高兴的事情。有些读者私信我说希望后面多分享spring方面的文章，这样能够在实际工作中派上用场。正好我对spring源码有过一定的研究，并结合我这几年实际的工作经验，把spring中我认为不错的知识点总结一下，希望对您有所帮助。 实现…",
		IsEnglish:    0,
		IsOriginal:   1,
		Content:      "",
		Ctime:        1613896165,
		Mtime:        1625579546,
		Rtime:        1613994419,
		ViewCount:    10374,
		CollectCount: 533,
		DiggCount:    339,
		CommentCount: 22,
		Status:       2,
	}
	str, _ := json.Marshal(article)
	fmt.Println(string(str))
	article2 := &Article{}
	_ = json.Unmarshal(str, article2)
	fmt.Println(article2)
}
```

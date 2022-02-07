---
title: Referrer-Policy
date: 2020-09-22 19:55:12
tags: []
---

这两天发现网页浏览量显示不对，深入调查后发现是`Chrome 85`对`Referrer-Policy`进行了调整，
将原来默认的`no-referrer-when-downgrade`调整为`strict-origin-when-cross-origin`。

## 什么是Referer

`Referer`请求头包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。
服务端一般使用`Referer`请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。

需要注意的是`referer`实际上是`referrer`误拼写。参见[HTTP referer on Wikipedia](https://zh.wikipedia.org/wiki/HTTP_referer)来获取更详细的信息。

## 什么是Referrer-Policy

`Referrer-Policy`首部用来监管哪些访问来源信息——会在`Referer`中发送——应该被包含在生成的请求当中。

* `no-referrer`: 整个`Referer`首部会被移除。访问来源信息不随着请求一起发送。
* `no-referrer-when-downgrade`: 在没有指定任何策略的情况下用户代理的默认行为。在同等安全级别的情况下，
  引用页面的地址会被发送(HTTPS->HTTPS)，但是在降级的情况下不会被发送(HTTPS->HTTP)。
* `origin`: 在任何情况下，仅发送文件的源作为引用地址。例如 https://example.com/page.html 会将 https://example.com/ 作为引用地址。
* `origin-when-cross-origin`: 对于同源的请求，会发送完整的URL作为引用地址，但是对于非同源请求仅发送文件的源。<!--more-->
* `same-origin`: 对于同源的请求会发送引用地址，但是对于非同源请求则不发送引用地址信息。
* `strict-origin`: 在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)，但是在降级的情况下不会发送(HTTPS->HTTP)。
* `strict-origin-when-cross-origin`: 对于同源的请求，会发送完整的URL作为引用地址；在同等安全级别的情况下，
  发送文件的源作为引用地址(HTTPS->HTTPS)；在降级的情况下不发送此首部(HTTPS->HTTP)。
* `unsafe-url`: 无论是同源请求还是非同源请求，都发送完整的URL（移除参数信息之后）作为引用地址。

## 应对Chrome 85的调整

两种方法：

* 为web服务器添加`Referrer-Policy`响应头，将值设为`no-referrer-when-downgrade`。
* 在html页面中添加`meta`元素：`<meta name="referrer" content="no-referrer-when-downgrade">`。

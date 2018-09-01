---
title: HTTP状态码
date: 2018-08-31 15:11:14
tags: [http]
---

HTTP状态码（HTTP Status Code）是用以表示网页服务器HTTP响应状态的3位数字代码。
它由[RFC2616][]规范定义的，并得到[RFC2518][]、[RFC2817][]、[RFC2295][]、[RFC2774][]、[RFC4918][]等规范扩展。

## 1xx: 信息

消息                     |描述
------------------------|------------------------------------------------------------------------------
100 Continue            |服务器仅接收到部分请求，但是一旦服务器并没有拒绝该请求，客户端应该继续发送其余的请求。
101 Switching Protocols |服务器转换协议：服务器将遵从客户的请求转换到另外一种协议。
102 Processing          |由WebDAV（[RFC2518][]）扩展的状态码，代表处理将被继续执行。

## 2xx: 成功

消息                     |描述
------------------------|------------------------------------------------------------------------------
200 OK                  |请求成功（其后是对GET和POST请求的应答文档。）
201 Created             |请求被创建完成，同时新的资源被创建。
202 Accepted            |供处理的请求已被接受，但是处理未完成。
203 Non-authoritative Information   |文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝。
204 No Content          |没有新文档。浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。
205 Reset Content       |没有新文档。但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容。
206 Partial Content     |客户发送了一个带有Range头的GET请求，服务器完成了它。
207 Multi-Status        |由WebDAV([RFC2518][])扩展的状态码，代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。

<!--more-->

## 3xx: 重定向

消息                     |描述
------------------------|------------------------------------------------------------------------------
300 Multiple Choices    |多重选择。链接列表。用户可以选择某链接到达目的地。最多允许五个地址。
301 Moved Permanently   |所请求的页面已经转移至新的url。
302 Found               |所请求的页面已经临时转移至新的url。
303 See Other           |所请求的页面可在别的url下被找到。
304 Not Modified        |未按预期修改文档。客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告诉客户，原来缓冲的文档还可以继续使用。
305 Use Proxy           |客户请求的文档应该通过Location头所指明的代理服务器提取。
306 Unused              |此代码被用于前一版本。目前已不再使用，但是代码依然被保留。
307 Temporary Redirect  |被请求的页面已经临时移至新的url。

## 4xx: 客户端错误

消息                     |描述
------------------------|------------------------------------------------------------------------------
400 Bad Request         |服务器未能理解请求。
401 Unauthorized        |被请求的页面需要用户名和密码。
402 Payment Required    |此代码尚无法使用。
403 Forbidden           |对被请求页面的访问被禁止。
404 Not Found           |服务器无法找到被请求的页面。
405 Method Not Allowed  |请求中指定的方法不被允许。
406 Not Acceptable      |服务器生成的响应无法被客户端所接受。
407 Proxy Authentication Required   |用户必须首先使用代理服务器进行验证，这样请求才会被处理。
408 Request Timeout     |请求超出了服务器的等待时间。
409 Conflict            |由于冲突，请求无法被完成。
410 Gone                |被请求的页面不可用。
411 Length Required     |"Content-Length" 未被定义。如果无此内容，服务器不会接受请求。
412 Precondition Failed |请求中的前提条件被服务器评估为失败。
413 Request Entity Too Large        |由于所请求的实体的太大，服务器不会接受请求。
414 Request-url Too Long            |由于url太长，服务器不会接受请求。当post请求被转换为带有很长的查询信息的get请求时，就会发生这种情况。
415 Unsupported Media Type          |由于媒介类型不被支持，服务器不会接受请求。
416 Requested Range Not Satisfiable |服务器不能满足客户在请求中指定的Range头。
417 Expectation Failed  |在请求头 Expect 中指定的预期内容无法被服务器满足，或者这个服务器是一个代理服务器，它有明显的证据证明在当前路由的下一个节点上，Expect 的内容无法被满足。
421 too many connections|从当前客户端所在的IP地址到服务器的连接数超过了服务器许可的最大范围。通常，这里的IP地址指的是从服务器上看到的客户端地址（比如用户的网关或者代理服务器地址）。在这种情况下，连接数的计算可能涉及到不止一个终端用户。
422 Unprocessable Entity|请求格式正确，但是由于含有语义错误，无法响应。（[RFC4918][] WebDAV）
423 Locked              |当前资源被锁定。（[RFC4918][] WebDAV）
424 Failed Dependency   |由于之前的某个请求发生的错误，导致当前请求失败，例如 PROPPATCH。（[RFC4918][] WebDAV）
425 Unordered Collection|在WebDav Advanced Collections 草案中定义，但是未出现在《WebDAV 顺序集协议》（[RFC3658][]）中。
426 Upgrade Required    |客户端应当切换到TLS/1.0。（[RFC2817][]）
449 Retry With          |由微软扩展，代表请求应当在执行完适当的操作后进行重试。
451 Unavailable For Legal Reasons   |该请求因法律原因不可用。（[RFC7725][]）

## 5xx: 服务器错误

消息                     |描述
------------------------|------------------------------------------------------------------------------
500 Internal Server Error           |请求未完成。服务器遇到不可预知的情况。
501 Not Implemented     |请求未完成。服务器不支持所请求的功能。
502 Bad Gateway         |请求未完成。服务器从上游服务器收到一个无效的响应。
503 Service Unavailable |请求未完成。服务器临时过载或当机。
504 Gateway Timeout     |网关超时。
505 HTTP Version Not Supported      |服务器不支持请求中指明的HTTP协议版本。
506 Variant Also Negotiates         |由《透明内容协商协议》（[RFC2295][]）扩展，代表服务器存在内部配置错误：被请求的协商变元资源被配置为在透明内容协商中使用自己，因此在一个协商处理中不是一个合适的重点。
507 Insufficient Storage            |服务器无法存储完成请求所必须的内容。这个状况被认为是临时的。WebDAV（[RFC4918][]）
509 Bandwidth Limit Exceeded        |服务器达到带宽限制。这不是一个官方的状态码，但是仍被广泛使用。
510 Not Extended        |获取资源所需要的策略并没有被满足。（[RFC2774][]）

## 6xx: 服务器错误

消息                     |描述
------------------------|------------------------------------------------------------------------------
600 Unparseable Response Headers    |源站没有返回响应头部，只返回实体内容


[RFC2616]: https://tools.ietf.org/html/rfc2616
[RFC2518]: https://tools.ietf.org/html/rfc2518
[RFC2817]: https://tools.ietf.org/html/rfc2817
[RFC2295]: https://tools.ietf.org/html/rfc2295
[RFC2774]: https://tools.ietf.org/html/rfc2774
[RFC4918]: https://tools.ietf.org/html/rfc4918
[RFC7725]: https://tools.ietf.org/html/rfc7725
[RFC3658]: https://tools.ietf.org/html/rfc3658

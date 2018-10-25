---
title: Basic Auth
date: 2018-10-23 09:00:31
tags: [auth]
---

Basic认证是HTTP中非常简单的认证方式，因为简单，所以不是很安全，不过仍然非常常用。

当一个客户端向一个需要认证的HTTP服务器进行数据请求时，
如果之前没有认证过，HTTP服务器会返回401状态码，要求客户端输入用户名和密码。
用户输入用户名和密码后，用户名和密码会经过BASE64加密附加到请求信息中再次请求HTTP服务器，
HTTP服务器会根据请求头携带的认证信息，决定是否认证成功及做出相应的响应。

Basic认证要求在请求时在请求头中添加`Authorization: Basic Base64编码串`，
`Base64编码串`生成规则是`user:password`做Base64编码。

curl使用`-u, --user <user:password>`设置Basic认证。

详见：[HTTP Authentication: Basic and Digest Access Authentication][RFC2617]

[RFC2617]: https://tools.ietf.org/html/rfc2617

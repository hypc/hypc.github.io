---
title: cannot-validate-certificate
date: 2021-08-25 13:00:13
tags:
---

> x509: cannot validate certificate for `<ipaddress>` because it doesn't contain any IP SANs

一般情况下，证书会包含一些信息，如`国家`、`机构`、`ip`、`域名`等等。
这个问题发生的时机在客户端访问服务器，如果证书中没有包含当前访问的`ip`、`域名`等信息时，就会产生这个问题。

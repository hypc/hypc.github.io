---
title: OAuth2 授权与 CSRF 攻击
date: 2021-12-08 11:09:47
tags: [oauth2, csrf]
---

我们在开发微服务的时候，多半会采用 OAuth2 进行授权认证。
本文将介绍如何通过状态随机数来防止可能的 CSRF 攻击。

## CSRF

{% post_link csrf "CSRF 攻击" %}

## OAuth2 中的 CSRF 攻击

OAuth2 授权过程中有以下几个参数：

* `response_type=code`
* `client_id`
* `redirect_uri`

这几个参数都是公开的，所有用户都使用相同的值。
攻击者也无法控制 `redirect_uri`，因为这个会配置在授权服务器的合法地址列表中。

攻击者能控制的是可选参数 `state`，如果授权过程中提供了 `state`，那么授权成功后会原封不动的返回 `state`。
授权流程如下：

```mermaid
sequenceDiagram
autonumber
participant A as Webapp
participant C as Passport
participant B as Backend

A ->> +C: redirect /login?...&state=value
C ->> C: login
C -->> -A: redirect /?code=...&state=value

A -->> +C: /TOKEN?code=...
C -->> -A: access_token

A ->> B: Authorization: Bearer <access_token>
```

`state` 本身不容易受到任何攻击，但是 Web 应用程序可能会实现自定义逻辑，这种逻辑使用攻击者有机可乘。
攻击流程如下：

```mermaid
sequenceDiagram
autonumber
participant A as Webapp
participant C as Passport
Actor D as Attacker
participant B as Backend

D ->> +C: redirect /login?...&state=value
C ->> C: login
C -->> -A: redirect /?code=...&state=value

A -->> +C: /TOKEN?code=...
C -->> -A: access_token

A ->> A: read state

A ->> B: CSRF Attack
```

<!--more-->

## 防御

要防止这种攻击也非常简单，将 `state` 换成随机数，应用程序在本地妥善保存这个随机数与业务之间的关系。
在授权结束后，通过随机数判断要执行的业务，并在同时将随机数与业务对应的关系删除。
而在每次请求时都使用新的随机数，这样便能防止攻击者猜测到真正要执行的业务。

```mermaid
sequenceDiagram
autonumber
participant Webapp
participant Passport
Actor Attacker
participant Backend

Webapp ->> +Passport: redirect /login?...&state=nonce
Passport ->> Passport: login
Passport -->> -Webapp: redirect /?code=...&state=nonce

Webapp -->> +Passport: /TOKEN?code=...
Passport -->> -Webapp: access_token

Webapp ->> Webapp: compare state nonce with stored
Webapp ->> Backend: Authorization: Bearer <access_token>

Note Over Webapp, Backend: CSRF Attack
Attacker ->> +Passport: redirect /login?...&state=nonce
Passport ->> Passport: login
Passport -->> -Webapp: redirect /?code=...&state=nonce

Webapp -->> +Passport: /TOKEN?code=...
Passport -->> -Webapp: access_token

Webapp ->> Webapp: no stored nonce
Note Over Webapp: Error
```

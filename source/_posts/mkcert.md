---
title: 使用mkcert创建自签名证书
date: 2021-01-31 19:07:41
tags: [mkcert, ssl]
---

在做本地开发的时候，免不了需要模拟https环境，这时候便需要使用自签名证书，自签名证书可以使用openssl生成，
但这一系列步骤过于复杂 -- 我们需要使用一种简单且友好的方式生成本地https证书，那便是[mkcert][]方案。

[mkcert]: https://github.com/FiloSottile/mkcert

## Installation

### MacOS

```bash
brew install mkcert
```

### Linux

```bash
sudo apt install libnss3-tools
# or
sudo yum install nss-tools
```

<!--more-->

## Usage

### 将CA证书加入本地可信CA

```bash
mkcert -install
```

### 生成自签证书

```bash
$ mkcert your.domain.com
Using the local CA at "/Users/hypc/Library/Application Support/mkcert" ✨

Created a new certificate valid for the following names 📜
 - "your.domain.com"

The certificate is at "./your.domain.com.pem" and the key at "./your.domain.com-key.pem" ✅
```

### 配置nginx

```nginx
server {
    listen      443 ssl;
    server_name your.domain.com;

    ssl_certificate     your.domain.com.pem;
    ssl_certificate_key your.domain.com-key.pem;

    ...
}
```

---
title: letsencrypt申请泛域名证书
date: 2019-12-11 08:31:45
tags: [letsencrypt, ssl]
---

```bash
docker run -it \
    -v /etc/letsencrypt:/etc/letsencrypt \
    certbot/certbot certonly \
    --preferred-challenges dns \
    --server https://acme-v02.api.letsencrypt.org/directory \
    --manual \
    -d *.your-domain.com
```

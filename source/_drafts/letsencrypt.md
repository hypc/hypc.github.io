---
title: letsencrypt申请泛域名证书
date: 2019-12-11 08:31:45
tags: [docker, letsencrypt, ssl]
---

使用`docker`的`certbot`镜像进行泛域名申请：

```bash
docker run -it \
    -v /etc/letsencrypt:/etc/letsencrypt \
    certbot/certbot certonly \
    --preferred-challenges dns \
    --server https://acme-v02.api.letsencrypt.org/directory \
    --manual \
    -d *.your-domain.com
```

注意，中间会提示你添加TXT解析记录，添加完成后回来继续即可。

![](/images/letsencrypt-01.png)

---
title: 使用cert-manager管理证书
date: 2021-07-28 18:30:23
tags: [kubernetes, cert-manager]
---

[cert-manager][] 是一个云原生证书管理开源项目，用于在 Kubernetes 集群中提供 HTTPS 证书并自动续期，支持 [Let’s Encrypt][] / [HashiCorp][] / [Vault][] 这些免费证书的签发。

[cert-manager]: https://cert-manager.io/
[Let’s Encrypt]: https://letsencrypt.org/
[HashiCorp]: https://www.hashicorp.com/
[Vault]: https://www.vault.com/

## 安装 cert-manager

```bash
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.4.0 \
  --set installCRDs=true
```

安装时必须设置`installCRDs=true`，否则后面安装`Issuer`会抛出下面错误：

> error: no matches for kind "Issuer" in version "cert-manager.io/v1"

<!--more-->

## 安装 Issuer

在安装`Issuer`之前，先去 [Cloudflare][] 申请 `API Token`：

[Cloudflare]: https://www.cloudflare.com/

![](/images/cert-manager.png)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: cloudflare-api-token-secret
  namespace: cert-manager
type: Opaque
stringData:
  api-token: xxx
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: cert-manager-issuer
spec:
  acme:
    email: name@example.com
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: cert-manager-issuer-account-key
    solvers:
    - dns01:
        cloudflare:
          email: name@example.com
          apiTokenSecretRef:
            name: cloudflare-api-token-secret
            key: api-token
```

## Sample

* {% post_link k3s-httpbin "在k3s上部署httpbin服务" %}

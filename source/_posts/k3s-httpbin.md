---
title: 在k3s上部署httpbin服务
date: 2021-07-30 16:43:59
tags: [kubernetes, k3s]
---

[httpbin][] 是一个 HTTP Request & Response Service，你可以向他发送请求，然后他会按照指定的规则将你的请求返回。

[httpbin]: https://httpbin.org/

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpbin
  labels:
    app: httpbin
spec:
  replicas: 3
  selector:
    matchLabels:
      app: httpbin
  template:
    metadata:
      labels:
        app: httpbin
    spec:
      containers:
        - name: httpbin
          image: kennethreitz/httpbin
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: httpbin
spec:
  selector:
    app: httpbin
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

<!--more-->

`Ingress` 配置如下：

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: httpbin
  annotations:
    cert-manager.io/cluster-issuer: cert-manager-issuer   # 使用 cert-manager 来获取证书
    kubernetes.io/ingress.class: traefik
spec:
  tls:
    - hosts:
        - httpbin.yourdomain.com
      secretName: httpbin-cert
  rules:
    - host: httpbin.yourdomain.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: httpbin
                port:
                  number: 80
```

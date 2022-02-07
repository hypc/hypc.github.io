---
title: 在k3s上部署longhorn服务
date: 2021-08-13 15:35:58
tags: [k3s, longhorn, kubernetes]
---

[Longhorn](https://longhorn.io/) 是 Kubernetes 的云原生分布式块存储。

## Install

注意：在安装 `longhorn` 之前，需要先在节点上安装 `iSCSI`：

```bash
yum install -y iscsi-initiator-utils targetcli
systemctl enable --now iscsid
```

然后安装 longhorn：

```bash
helm repo add longhorn https://charts.longhorn.io
kubectl create namespace longhorn-system
helm install longhorn longhorn/longhorn --namespace longhorn-system
```

<!--more-->

## Exposing Dashbord

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: longhorn
  namespace: longhorn-system
  annotations:
    cert-manager.io/cluster-issuer: cert-manager-issuer
    kubernetes.io/ingress.class: traefik
spec:
  tls:
    - hosts:
        - longhorn.yourdomain.com
      secretName: longhorn-cert
  rules:
    - host: longhorn.yourdomain.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: longhorn-frontend
                port:
                  number: 80
```

下面是一个带有 `BasicAuth` 验证的 `Ingress`：

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: longhorn-auth
  namespace: longhorn-system
data:
  # `htpasswd -nb user password | openssl base64`
  users: dXNlcjokYXByMSRKU1dpTTN1ayRWVmhqQ3c4VnFlSDRpd1B3Q3l6UWwxCgo=
---
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: longhorn-auth
  namespace: longhorn-system
spec:
  basicAuth:
    secret: longhorn-auth
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: longhorn
  namespace: longhorn-system
  annotations:
    cert-manager.io/cluster-issuer: cert-manager-issuer
    kubernetes.io/ingress.class: traefik
    # 指定中间件名称，如果有多个，则使用 `,` 分开，配置规则：{namespace}-{middleware-name}@{资源类型}
    traefik.ingress.kubernetes.io/router.middlewares: longhorn-system-longhorn-auth@kubernetescrd
spec:
  tls:
    - hosts:
        - longhorn.local.hypc.host
      secretName: longhorn-cert
  rules:
    - host: longhorn.local.hypc.host
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: longhorn-frontend
                port:
                  number: 80
```

## Usage

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: longhorn-test-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: longhorn
  resources:
    requests:
      storage: 100Mi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: longhorn-test
  labels:
    app: longhorn-test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: longhorn-test
  template:
    metadata:
      labels:
        app: longhorn-test
    spec:
      containers:
        - name: longhorn-test
          image: nginx:stable-alpine
          volumeMounts:
            - name: data
              mountPath: /data
      volumes:
        - name: data
          persistentVolumeClaim:
            claimName: longhorn-test-pvc
```

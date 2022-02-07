---
title: 搭建k3s集群
date: 2021-07-06 15:21:32
tags: [k3s, kubernets]
---

[k3s][]是经CNCF一致性认证的Kubernetes发行版，专为物联网及边缘计算设计。

[k3s]: https://rancher.com/products/k3s/

## 集群规划

服务器      | 用途                    | 配置  | IP
-----------|-------------------------|------|----------------
k3s-master | k3s-server（master节点） | 2C2G | 10.17.56.20
k3s-agent1 | k3s-agent（agent节点1）  | 2C2G | 10.17.56.21
k3s-agent2 | k3s-agent（agent节点2）  | 2C2G | 10.17.56.22

这三台机器安装的是`CentOS7`系统。

<!--more-->

## 安装k3s master节点

```bash
yum makecache \
    && yum update -y \
    && yum install -y git vim tree yum-utils \
    && curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /usr/local/bin/jq \
    && chmod a+x /usr/local/bin/jq

echo 'vm.max_map_count = 655300' >> /etc/sysctl.conf && sysctl -p
systemctl stop firewalld.service && systemctl disable firewalld.service

mkdir -p /etc/rancher/k3s/ && tee /etc/rancher/k3s/registries.yaml <<"EOF"
mirrors:
  docker.io:
    endpoint:
      - "https://xxx.mirror.aliyuncs.com"
  quay.io:
    endpoint:
      - "https://quay.mirrors.yourproxy.com"
EOF

export K3S_NODE_NAME=k3s-master
export K3S_EXTERNAL_IP=10.17.56.20
export INSTALL_K3S_MIRROR=cn
curl -sfL http://rancher-mirror.cnrancher.com/k3s/k3s-install.sh | sh -
```

## 安装k3s agent节点

安装agent节点前，需要先登陆master节点，获取`node-token`：

```bash
cat /var/lib/rancher/k3s/server/node-token
```

然后登陆agent节点，执行以下命令：

```bash
yum makecache \
    && yum update -y \
    && yum install -y git vim tree yum-utils \
    && curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 -o /usr/local/bin/jq \
    && chmod a+x /usr/local/bin/jq

echo 'vm.max_map_count = 655300' >> /etc/sysctl.conf && sysctl -p
systemctl stop firewalld.service && systemctl disable firewalld.service

mkdir -p /etc/rancher/k3s/ && tee /etc/rancher/k3s/registries.yaml <<"EOF"
mirrors:
  docker.io:
    endpoint:
      - "https://xxx.mirror.aliyuncs.com"
  quay.io:
    endpoint:
      - "https://quay.mirrors.yourproxy.com"
EOF

export K3S_URL=https://10.17.56.20:6443
export K3S_TOKEN=       # 在master节点上获取的`node-token`
export K3S_NODE_NAME=k3s-agent1
# export K3S_NODE_NAME=k3s-agent2
export INSTALL_K3S_MIRROR=cn

curl -sfL http://rancher-mirror.cnrancher.com/k3s/k3s-install.sh | sh -
```

## 管理集群

[lens][]是一个开源的管理`Kubernetes`集群的IDE，支持`MacOS`，`Windows`和`Linux`。

[lens]: https://k8slens.dev/

1. 先登陆master节点，复制`/etc/rancher/k3s/k3s.yaml`文件中的内容。
2. 打开[lens][]，点击`添加集群`，将刚刚复制的内容粘贴到里面，修改`server`地址，保存即可。
3. 启用`metrics`，选中刚刚添加的集群，依次点击`settings -> LENS METRICS`，将其中选项全部开启，保存，等待一段时间之后便能看到`metrics`数据了。

关于[k3s][]、[lens][]更多用法请自行摸索。

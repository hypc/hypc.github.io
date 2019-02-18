---
title: kubectl常用用法
date: 2019-02-14 11:42:45
tags: [kubernetes, docker]
---

kubectl是Kubernetes集群管理器的命令行工具，使用kubectl可以在Kubernetes上部署和管理应用程序。
使用kubectl，可以检查集群资源；创建，删除和更新组件。

更多kubectl参考：https://kubernetes.io/docs/reference/kubectl/overview/

## kubectl常用命令

### 查看集群信息

```bash
kubectl cluster-info
```

### 查看资源信息

```bash
kubectl get nodes       # 查看node信息
kubectl get pods        # 查看pod信息
kubectl get rc,service  # 同时查看rc和service信息
```

<!--more-->

### 查看资源的详细信息

```bash
# kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME) [options]
kubectl describe nodes <node_name>  # 查看node的详细信息
kubectl describe pods/<pod_name>    # 查看pod的详细信息
```

### 创建资源信息

```bash
# kubectl create -f FILENAME [options]
kubectl create -f <my_resource_filename>     # 根据配置文件创建资源
kubectl create -f <my_resource_directory>    # 根据配置文件所在目录创建资源
kubectl create -f <my_resource_url>          # 根据配置文件url创建资源
```

### 删除资源对象

```bash
# kubectl delete ([-f FILENAME] | TYPE [(NAME | -l label | --all)]) [options]
kubectl delete -f FILENAME          # 根据配置文件删除资源
kubectl delete pod,service baz foo  # 删除名为「baz」、「foo」的pod和service
kubectl delete pods --all           # 删除所有的pod
```

### 执行容器命令

```bash
# kubectl exec POD [-c CONTAINER] -- COMMAND [args...] [options]
kubectl exec <pod-name> data                            # 执行Pod的data命令，默认是用Pod中的第一个容器执行
kubectl exec <pod-name> -c <container-name> data        # 执行Pod的data命令
kubectl exec -it <pod-name> -c <container-name> bash    # 通过bash获得Pod中某个容器的TTY，相当于登录容器
```

## kubectl命令列表

```bash
$ kubectl help
kubectl controls the Kubernetes cluster manager.

Find more information at: https://kubernetes.io/docs/reference/kubectl/overview/

Basic Commands (Beginner):
  create         Create a resource from a file or from stdin.
  expose         Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
  run            Run a particular image on the cluster
  set            Set specific features on objects

Basic Commands (Intermediate):
  explain        Documentation of resources
  get            Display one or many resources
  edit           Edit a resource on the server
  delete         Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout        Manage the rollout of a resource
  scale          Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
  autoscale      Auto-scale a Deployment, ReplicaSet, or ReplicationController

Cluster Management Commands:
  certificate    Modify certificate resources.
  cluster-info   Display cluster info
  top            Display Resource (CPU/Memory/Storage) usage.
  cordon         Mark node as unschedulable
  uncordon       Mark node as schedulable
  drain          Drain node in preparation for maintenance
  taint          Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
  describe       Show details of a specific resource or group of resources
  logs           Print the logs for a container in a pod
  attach         Attach to a running container
  exec           Execute a command in a container
  port-forward   Forward one or more local ports to a pod
  proxy          Run a proxy to the Kubernetes API server
  cp             Copy files and directories to and from containers.
  auth           Inspect authorization

Advanced Commands:
  diff           Diff live version against would-be applied version
  apply          Apply a configuration to a resource by filename or stdin
  patch          Update field(s) of a resource using strategic merge patch
  replace        Replace a resource by filename or stdin
  wait           Experimental: Wait for a specific condition on one or many resources.
  convert        Convert config files between different API versions

Settings Commands:
  label          Update the labels on a resource
  annotate       Update the annotations on a resource
  completion     Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  api-resources  Print the supported API resources on the server
  api-versions   Print the supported API versions on the server, in the form of "group/version"
  config         Modify kubeconfig files
  plugin         Provides utilities for interacting with plugins.
  version        Print the client and server version information

Usage:
  kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
```

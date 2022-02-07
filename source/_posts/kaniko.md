---
title: Kaniko
date: 2021-09-25 14:42:52
tags: [kaniko]
---

[Kaniko][] 是一种在容器或 `Kubernetes` 集群内从 `Dockerfile` 构建容器镜像的工具。

[Kaniko][] 不依赖于 `Docker` 守护进程，而是完全在用户空间中执行 `Dockerfile` 中的每个命令。
这使得在无法轻松或安全地运行 `Docker` 守护程序的环境中构建容器镜像成为可能，例如标准的 `Kubernetes` 集群。

[Kaniko]: https://github.com/GoogleContainerTools/kaniko

下面是一个使用 Docker 构建并将镜像上传到 `gcr.io` 服务器的示例：

```bash
docker run \
    -v "$HOME"/.config/gcloud:/root/.config/gcloud \
    -v /path/to/context:/workspace \
    gcr.io/kaniko-project/executor:latest \
    --dockerfile /workspace/Dockerfile \
    --destination "gcr.io/$PROJECT_ID/$IMAGE_NAME:$TAG" \
    --context dir:///workspace/
```

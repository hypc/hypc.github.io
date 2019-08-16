---
title: Go项目代码组织结构
date: 2019-08-16 09:46:17
tags: [golang]
---

## 工作空间

Go工作空间下有三个子目录：

* `src`: 存放源代码
* `pkg`: 编译后生成的文件
* `bin`: 编译后生成的可执行文件

原则上，项目代码都应该放在`$GOPATH`目录下，并且按照`$GOPATH/$git_url`路径存放项目代码。

例如，我有一个`go-demos`的项目，存放路径为`$GOPATH/github.com/hypc/go-demos/`。

## 项目目录结构

目录结构基本上就是一个项目的门面，很多时候我们从目录结构中就能够看出开发者对这门语言是否有足够的经验，
所以在这里首先要介绍的最佳实践就是如何在Go语言的项目或者服务中组织代码。

官方并没有给出一个推荐的目录划分方式，很多项目对于目录结构的划分也非常随意，
但是社区中还是有一些比较常见的约定，例如：[`golang-standards/project-layout`][]项目中就定义了一个比较标准的目录结构。

<!--more-->

```txt
api/            -- OpenAPI/Swagger specs, JSON schema files, protocol definition files.
assets/         -- Other assets to go along with your repository (images, logos, etc).
build/          -- Packaging and Continuous Integration.
cmd/            -- Main applications for this project.
configs/        -- Configuration file templates or default configs.
deployments/    -- IaaS, PaaS, system and container orchestration deployment configurations and templates (docker-compose, kubernetes/helm, mesos, terraform, bosh).
docs/           -- Design and user documents (in addition to your godoc generated documentation).
examples/       -- Examples for your applications and/or public libraries.
githooks/       -- Git hooks.
init/           -- System init (systemd, upstart, sysv) and process manager/supervisor (runit, supervisord) configs.
internal/       -- Private application and library code. This is the code you don't want others importing in their applications or libraries.
    |-- app/    -- application code can go in the `/internal/app` directory
    |-- pkg/    -- the code shared by those apps in the `/internal/pkg` directory
pkg/            -- Library code that's ok to use by external applications (e.g., `/pkg/mypubliclib`). Other projects will import these libraries expecting them to work, so think twice before you put something here :-)
scripts/        -- Scripts to perform various build, install, analysis, etc operations.
test/           -- Additional external test apps and test data.
third_party/    -- External helper tools, forked code and other 3rd party utilities (e.g., Swagger UI).
tools/          -- Supporting tools for this project. Note that these tools can import code from the `/pkg` and /internal directories.
vendor/         -- Application dependencies (managed manually or by your favorite dependency management tool like dep).
web/            -- Web application specific components: static web assets, server side templates and SPAs.
website/        -- This is the place to put your project's website dapopta if you are not using Github pages.
.gitignore      -- gitignore
LICENSE.md      -- LICENSE
Makefile        -- call scripts from `/scripts`
README.md       -- README
```

[`golang-standards/project-layout`]: https://github.com/golang-standards/project-layout/

### /pkg

`/pkg`目录是Go语言项目中非常常见的目录，这个目录中存放的就是项目中可以被外部应用使用的代码库，
其他的项目可以直接通过`import`引入这里的代码，所以当我们将代码放入`pkg`时一定要慎重。

### /internal

私有代码推荐放到`/internal`目录中，真正的项目代码应该写在`/internal/app`里，
同时这些内部应用依赖的代码库应该在`/internal/pkg`子目录和`/pkg`中。

### /cmd

`/cmd`目录中存储的都是当前项目中的可执行文件，该目录下的每一个子目录都应该包含我们希望有的可执行文件。

### /api

`/api`目录中存放的就是当前项目对外提供的各种不同类型的API接口定义文件，
其中可能包含类似`/api/protobuf-spec`、`/api/thrift-spec`、`/api/swagger-spec`的子目录。

### /configs

`/configs`目录存放默认配置文件或配置模板文件。

### /scripts

`/scripts`存放用于构建、安装、分析等操作的脚本文件。

---
title: Lerna
date: 2021-11-23 15:10:03
tags: [lerna]
---

[Lerna][] 是一个管理工具，用于管理包含多个软件包（package）的 JavaScript 项目。

[Lerna][] 有两种工作模式：

* **Fixed/Locked mode** (default): 所有的包共用一个版本号。
* **Independent mode**: 使用 `--independent, -i` 参数初始化项目，每个包单独指定版本号。

[Lerna]: https://lerna.js.org/

## Getting Started

初始化项目：

```bash
$ mkdir lerna-demo && cd $_
$ npm install -D lerna
$ npx lerna init
```

目录结构如下：

```txt
lerna-demo/
|-- packages/
|-- package.json
|-- lerna.json
```

创建包：

```bash
$ npx lerna create @lerna-demo/package1
```

最终目录结构如下：

```txt
lerna-demo/
|-- packages/
    |-- package1/
        |-- __tests__/
            |-- package1.test.js
        |-- lib/
            |-- package1.js
        |-- package.json
        |-- README.md
|-- package.json
|-- lerna.json
```

当然，除了可以使用 `lerna create <pkg-name>` 创建 `package` 之外，还可以使用其他 `cli` 工具自行创建 `package`。

<!--more-->

## Commands

* [`lerna publish`](https://github.com/lerna/lerna/blob/main/commands/publish#readme)
* [`lerna version`](https://github.com/lerna/lerna/blob/main/commands/version#readme)
* [`lerna bootstrap`](https://github.com/lerna/lerna/blob/main/commands/bootstrap#readme)
* [`lerna list`](https://github.com/lerna/lerna/blob/main/commands/list#readme)
* [`lerna changed`](https://github.com/lerna/lerna/blob/main/commands/changed#readme)
* [`lerna diff`](https://github.com/lerna/lerna/blob/main/commands/diff#readme)
* [`lerna exec`](https://github.com/lerna/lerna/blob/main/commands/exec#readme)
* [`lerna run`](https://github.com/lerna/lerna/blob/main/commands/run#readme)
* [`lerna init`](https://github.com/lerna/lerna/blob/main/commands/init#readme)
* [`lerna add`](https://github.com/lerna/lerna/blob/main/commands/add#readme)
* [`lerna clean`](https://github.com/lerna/lerna/blob/main/commands/clean#readme)
* [`lerna import`](https://github.com/lerna/lerna/blob/main/commands/import#readme)
* [`lerna link`](https://github.com/lerna/lerna/blob/main/commands/link#readme)
* [`lerna create`](https://github.com/lerna/lerna/blob/main/commands/create#readme)
* [`lerna info`](https://github.com/lerna/lerna/blob/main/commands/info#readme)

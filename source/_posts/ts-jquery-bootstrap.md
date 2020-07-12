---
title: 在TypeScript中使用jQuery和BootStrap
date: 2020-06-02 09:13:23
tags: [javascript]
---

## 在项目使用jQuery和BootStrap

1. 首先安装[jQuery][]和[BootStrap][]：

    ```bash
    npm install jquery bootstrap
    ```

2. 然后在入口页面（一般是首页）中引入[BootStrap][]：

    ```ts
    import 'bootstrap/dist/css/bootstrap.min.css'
    import 'bootstrap/dist/js/bootstrap.min.js'
    ```

3. 在需要使用[jQuery][]的页面引入[jQuery][]：

    ```ts
    import $ from 'jquery'
    ```

<!--more-->

## 使用`@types`

[DefinitelyTyped][]是一个高质量的[TypeScript][]类型定义的仓库。

该社区已经记录了`90%`的顶级JavaScript库，这意味着，你可以非常高效地使用这些库，而无须在单独的窗口打开相应文档以确保输入的正确性。

1. 安装声明文件：

    ```bash
    npm install --save-dev @types/jquery @types/bootstrap
    ```

2. 然后配置`tsconfig.json`的`compilerOptions.types`选项：

    ```json
    {
        "compilerOptions": {
            "types" : [
                "jquery",
                "bootstrap"
            ]
        }
    }
    ```

[TypeScript]: https://www.typescriptlang.org/
[jQuery]: https://jquery.com/
[BootStrap]: https://www.bootcss.com/
[DefinitelyTyped]: https://github.com/DefinitelyTyped/DefinitelyTyped

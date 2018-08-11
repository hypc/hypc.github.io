---
title: Angular2多环境开发
date: 2018-08-11 09:33:03
tags: [angular]
---

## 配置环境变量文件

假设需要配置一个新的环境`NAME`。

首先配置一个新环境的配置文件，配置文件是：`./src/environments/environment.NAME.ts`。

## 设置`.angular-cli.json`文件

在`.angular-cli.json`中的`apps[0].environments`下配置新的环境：

```json
{
    "NAME": "environment.NAME.ts"
}
```

## 开发调试

开发调试可以使用命令：

```bash
ng serve --target=development --environment=NAME
```

## 部署

开发完可以使用下面命令部署：

```bash
ng build --target=production --environment=NAME
```

## 其他

为了简化开发调试的命令，可以在`package.json`文件中的`scripts`下添加以下内容：

```json
{
    "NAME": "ng serve --target=development --environment=NAME"
}
```

在开发调试时可以使用命令：`npm run NAME`。

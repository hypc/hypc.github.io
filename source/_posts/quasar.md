---
title: Quasar入门
date: 2021-05-30 13:14:09
tags: [quasar, vuejs, nodejs]
---

* 基于Vue.js
* 开箱即用的提供给网站和应用程序的最先进的UI（遵循《Material指南》）
* 开箱即用的对桌面和移动浏览器（包括iOS Safari！）的最佳支持
* 通过与我们自己的CLI紧密集成，对每种构建模式（SPA、SSR、PWA、移动应用程序、桌面应用程序和浏览器扩展）提供了一流的支持，并提供了最佳的开发人员体验
* 易于自定义（CSS）和可扩展（JS）
* 这是最注重性能的框架
* 自动tree shaking
* 在我们的论坛和Discord聊天基础上的很棒的社区
* 具有包括新功能在内的定期发布周期
* 获得快速修复并听取社区的要求
* 处理整个开发经验（甚至包括创建应用程序的图标和启动画面）

[Quasar][]是基于Vue的第一解决方案，无论您是仅构建桌面网站、桌面应用还是移动应用，或者所有这些。

[Quasar]: https://quasar.dev/
[quasarchs]: http://www.quasarchs.com/

<!--more-->

使用`Quasar CLI`可以无缝的构建以下应用：

* SPAs (单页应用)
* SSR (服务器端渲染的应用) (+可选的PWA客户端接管)
* PWA（渐进式网页应用）
* BEX (浏览器扩展)
* 通过Cordova或Capacitor构建移动APP（Android、iOS…）
* 多平台桌面应用（使用Electron）

## Quasar CLI

### 安装

```bash
yarn global add @quasar/cli
# or
npm install -g @quasar/cli
```

### 创建项目

```bash
quasar create <project_name>
```

<video controls autoplay src="/images/quasar.mp4" style="max-width: 90%;"></video>

### 目录结构

这是安装了所有模式的项目的结构：

```
.
├── public/                  # 纯静态资源（直接复制）
├── src/
│   ├── assets/              # 动态资源（由webpack处理）
│   ├── components/          # 用于页面和布局的.vue组件
│   ├── css/                 # CSS/Stylus/Sass/...文件
|   |   ├── app.styl
|   │   └── quasar.variables.styl # 供您调整的Quasar Stylus变量
│   ├── layouts/             # 布局 .vue 文件
│   ├── pages/               # 页面 .vue 文件
│   ├── boot/                # 启动文件 (应用初始化代码)
│   ├── router/              # Vue路由
|   |   ├── index.js         # Vue路由定义
|   │   └── routes.js        # App路由定义
│   ├── store/               # Vuex Store
|   |   ├── index.js         # Vuex Store 定义
|   │   ├── <folder>         # Vuex Store 模块...
|   │   └── <folder>         # Vuex Store 模块...
│   ├── App.vue              # APP的根Vue组件
│   └── index.template.html  # index.html模板
├── src-ssr/                 # SSR特定代码(就像生产环境的Node网页服务器)
├── src-pwa/                 # PWA特定代码（如Service Worker）
├── src-cordova/             # Cordova生成的文件夹用于创建移动APP
├── src-electron/            # Electron特定代码（如"main"线程)
├── src-bex/                 # BEX（浏览器扩展）特定代码（如"main"线程)
├── dist/                    # 生产版本代码，用于部署
│   ├── spa/                 # 构建SPA的例子
│   ├── ssr/                 # 构建SSR的例子
│   ├── electron/            # 构建Electron的例子
│   └── ....
├── quasar.conf.js           # Quasar App配置文件
├── babel.config.js          # Babeljs配置
├── .editorconfig            # editor配置
├── .eslintignore            # ESlint忽略路径
├── .eslintrc.js             # ESlint配置
├── .postcssrc.js            # PostCSS配置
├── .stylintrc               # Stylus lint配置
├── .gitignore               # GIT忽略路径
├── package.json             # npm脚本和依赖项
└── README.md                # 您的网站/应用程序的自述文件
```

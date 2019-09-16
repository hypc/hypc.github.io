---
title: nodejs实现网站截图
date: 2018-07-24 20:20:53
tags: [nodejs]
---

这里介绍使用[pageres][]来进行网站截图。

## pageres

**安装**

```bash
npm install pageres
```

**使用**

```js
const Pageres = require('pageres');

const pageres = new Pageres({delay: 2})
    .src('yeoman.io', ['480x320', '1024x768', 'iphone 5s'], {crop: true})
    .src('todomvc.com', ['1280x1024', '1920x1080'])
    .src('data:text/html;base64,PGgxPkZPTzwvaDE+', ['1024x768'])
    .dest(__dirname)
    .run()
    .then(() => console.log('done'));
```

具体使用参考：[pageres][]

## pageres-cli

**安装**

```bash
npm install --global pageres-cli
```

**使用**

```bash
# pageres <url> <resolution>
pageres todomvc.com 1024x768 1366x768 # 2 screenshots
pageres todomvc.com yeoman.io 1024x768 # 2 screenshots
pageres todomvc.com yeoman.io 1024x768 1366x768 # 4 screenshots

# pageres [ <url> <resolution> ] [ <url> <resolution> ]
pageres [ yeoman.io 1024x768 1600x900 ] todomvc.com 1366x768
pageres [ yeoman.io 1024x768 --no-crop ] todomvc.com 1366x768 --crop
```

具体使用参考：[pageres-cli][]


[pageres]: https://github.com/sindresorhus/pageres
[pageres-cli]: https://github.com/sindresorhus/pageres-cli

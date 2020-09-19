---
title: 使用Webpack DevServer时捕获「Invalid Host header」异常
date: 2020-09-10 18:01:17
tags: [webpack, nodejs]
---

当我使用`nginx`反向代理到`vue service`时，产生了一个`Invalid Host header`异常。

经过查找，发现对`Host Header`检查是[Webpack DevServer][]的一种特性。

可以通过设置[devServer.disableHostCheck][]或[devServer.allowedHosts][]这两种方式中的一种来消除异常。

```js
module.exports = {
    //...
    devServer: {
        disableHostCheck: true,
        allowedHosts: ['.host.com', 'host2.com']
    }
}
```

其他有关使用[Webpack DevServer][]的服务都可以使用相同的办法处理。

[devServer.allowedHosts]: https://webpack.js.org/configuration/dev-server/#devserverallowedhosts
[devServer.disableHostCheck]: https://webpack.js.org/configuration/dev-server/#devserverdisablehostcheck
[Webpack DevServer]: https://webpack.js.org/configuration/dev-server/

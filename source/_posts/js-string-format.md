---
title: js字符串格式化
date: 2018-07-06 15:40:58
tags: [javascript]
---

```js
String.prototype.format = function(args) {
    if (arguments.length == 0) {
        return this;
    }
    var result = this;
    if (arguments.length == 1 && typeof(args) == "object") {
        for (var key in args) {
            if (args[key] != undefined) {
                var reg = new RegExp("({" + key + "})", "g");
                result = result.replace(reg, args[key]);
            }
        }
    } else {
        for (var i = 0; i < arguments.length; i++) {
            if (arguments[i] != undefined) {
                var reg = new RegExp("({)" + i + "(})", "g");
                result = result.replace(reg, arguments[i]);
            }
        }
    }
    return result;
}

var template1 = "Hello, {0}!";
var template2 = "Hello, {name}!";
console.log(template1.format("Tom"));
console.log(template2.format({name: "Tom"}));
```

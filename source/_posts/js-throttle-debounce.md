---
title: js函数节流和函数去抖
date: 2018-07-23 16:51:05
tags: [javascript]
---

## 函数节流

**函数节流**：就是函数按照一个周期执行，例如给window绑定一个resize事件之后，只要window大小改变就打印1，
如果不采用函数节流，当我们将窗口调节的时候发现控制台一直打印1，但是使用了函数节流后我们会发现调节的过程中，每隔一段时间才打印1。

```javascript
var throttle = function(delay, cb) {
    var startTime = Date.now();
    return function() {
        var currTime = Date.now();
        if (currTime - startTime > delay) {
            cb();
            startTime = currTime;
        }
    }
}
```

## 函数去抖

**函数去抖**：就是当事件触发之后，必须等待某一个时间之后，回调函数才会执行，假若再等待的时间内，事件又触发了则等待时间刷新。
还是上面那个例子，如果我们一直改变窗口大小，则不会打印1，只有当我们停止改变窗口大小并等待一段时间后，才会打印1。

```javascript
var debounce = function(delay, cb) {
    var timer;
    return function() {
        if (timer) clearTimeout(timer);
        timer = setTimeout(function() {
            cb();
        }, delay);
    }
}
```

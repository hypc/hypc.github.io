---
title: js缓存化技术
date: 2018-07-23 17:08:10
tags: [javascript]
---

`Memoization`是一种将函数返回值缓存起来的方法。

`Memoization`原理非常简单，就是把函数的每次执行结果都放入一个散列表（或数组）中，在接下来的执行中，
在散列表中查找是否已经有相应执行过的值，如果有，直接返回该值，没有才真正执行函数体的求值部分。
很明显，找值，尤其是在散列中找值，比执行函数快多了。现代JavaScript的开发也已经大量使用这种技术。

```javascript
var fib = (function() {
    var cache = [
        1, 1
    ];
    var f = function(num) {
        if (num <= 0)
            throw new Error("Num not less than 1.");
        if (num >= cache.length) {
            for (var i = cache.length; i <= num; i++) {
                cache[i] = cache[i - 2] + cache[i - 1];
            }
        }
        return cache[num - 1];
    };
    return f;
})();
```

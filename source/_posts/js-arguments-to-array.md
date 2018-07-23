---
title: js将arguments对象转换为数组
date: 2018-07-23 16:41:41
tags: [javascript]
---

```javascript
Array.prototype.slice.call(arguments, 0);
```

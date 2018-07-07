---
title: js解析url中的参数
date: 2018-07-07 16:24:27
tags: [javascript]
---

## 方法一

```javascript
function getQueryString(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) return decodeURIComponent(r[2]); return null;
}
```

**使用**

```javascript
console.log(getQueryString(name))
```

## 方法二

``` javascript
function getQueryParams() {
   var search = location.search; //获取url中"?"符后的字串
   var theRequest = new Object();
   if (search.indexOf("?") != -1) {
      var search = search.substr(1);
      params = search.split("&");
      for(var i = 0; i < params.length; i ++) {
         theRequest[params[i].split("=")[0]] = decodeURIComponent(params[i].split("=")[1]);
      }
   }
   return theRequest;
}
```

**使用**

```javascript
console.log(getQueryParams())
```

---
title: js添加月份
date: 2018-07-25 16:48:21
tags: [javascript]
---

## 版本1

```js
var isLeapYear = function(year) {
    return (year % 4 == 0) && (year % 100 != 0 || year % 400 == 0);
}

var addMonths = (function(){
    var DAYS_IN_MONTH = [
        [0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
        [0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    ];
    return function(date, m){
        var n = date.getFullYear() * 12 + date.getMonth();
        n += m;
        var ryear = parseInt(n / 12);
        var rmonth = n % 12 + 1;
        var rday = date.getDate();

        var days_in_month = DAYS_IN_MONTH[isLeapYear(date.getFullYear()) ? 1 : 0];
        if (rday > days_in_month[rmonth]){
            rday = days_in_month[rmonth];
        }

        var d = new Date(ryear, rmonth - 1, rday);
        d.setHours(date.getHours());
        d.setMinutes(date.getMinutes());
        d.setSeconds(date.getSeconds());
        d.setMilliseconds(date.getMilliseconds());
        return d;
    }
}());
```

<!--more-->

## 版本2

```js
Date.prototype.isLeapYear = function() {
    var year = this.getFullYear();
    return (year % 4 == 0) && (year % 100 != 0 || year % 400 == 0);
}

Date.prototype.addMonths = (function(){
    var DAYS_IN_MONTH = [
        [0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
        [0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    ];
    return function(m){
        var self = this;
        var n = self.getFullYear() * 12 + self.getMonth();
        n += m;
        var ryear = parseInt(n / 12);
        var rmonth = n % 12 + 1;
        var rday = self.getDate();

        var days_in_month = DAYS_IN_MONTH[self.isLeapYear() ? 1 : 0];
        if (rday > days_in_month[rmonth]){
            rday = days_in_month[rmonth];
        }

        var d = new Date(ryear, rmonth - 1, rday);
        d.setHours(self.getHours());
        d.setMinutes(self.getMinutes());
        d.setSeconds(self.getSeconds());
        d.setMilliseconds(self.getMilliseconds());
        return d;
    }
}());
```

---
title: TypeScript扩展Date对象
date: 2020-07-07 16:13:53
tags: [typescript, javascript]
---

今天做代码检查的时候发现，关于时间处理这一块可以做进一步提取。

最初的想法是，自定义一个类，继承`Date`，运行的时候抛出了一个很奇怪的错误：

```
Uncaught TypeError: this is not a Date object.
```

经过查找，发现有人也遇到了相同的问题：
[如何继承Date对象？](http://www.dailichun.com/2018/01/15/howtoextenddate.html)。

最终采用原型扩展的方式实现，代码如下：<!--more-->

```typescript
declare global {
  interface Date {
    fmt: (format: string) => string;
    addDays: (days: number) => Date;
    isToday: () => boolean;
    clone: () => Date;
    isAnotherMonth: (date: Date) => boolean;
    isWeekend: () => boolean;
    isSameDate: (date: Date) => boolean;
    isLeapYear: () => boolean;
  }
}

/* eslint no-extend-native: ["error", { "exceptions": ["Date"] }] */
Date.prototype.fmt = function (format = 'YYYY-MM-DD hh:mm:ss'): string {
  const o: { [key: string]: number } = {
    'Y+': this.getFullYear(), // year
    'M+': this.getMonth() + 1, // month
    'D+': this.getDate(), // day
    'h+': this.getHours(), // hour
    'm+': this.getMinutes(), // minute
    's+': this.getSeconds(), // second
    q: Math.floor((this.getMonth() + 3) / 3), // quarter
    S: this.getMilliseconds() // millisecond
  }

  if (/(Y+)/.test(format)) {
    format = format.replace(RegExp.$1, `${o['Y+']}`.substr(4 - RegExp.$1.length))
  }

  for (const k in o) {
    if (new RegExp(`(${k})`).test(format)) {
      const value = RegExp.$1.length > 1 && o[k] < 10 ? `0${o[k]}` : o[k]
      format = format.replace(RegExp.$1, value as string)
    }
  }
  return format
}

Date.prototype.addDays = function (days: number): Date {
  if (days) {
    this.setDate(this.getDate() + days)
  }
  return this
}

Date.prototype.isToday = function (): boolean {
  return this.isSameDate(new Date())
}

Date.prototype.clone = function (): Date {
  return new Date(+this)
}

Date.prototype.isAnotherMonth = function (date: Date): boolean {
  return date && this.getMonth() !== date.getMonth()
}

Date.prototype.isWeekend = function (): boolean {
  return this.getDay() === 0 || this.getDay() === 6
}

Date.prototype.isSameDate = function (date: Date): boolean {
  return date && this.getFullYear() === date.getFullYear() && this.getMonth() === date.getMonth() && this.getDate() === date.getDate()
}

Date.prototype.isLeapYear = function (): boolean {
  const year = this.getFullYear()
  return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0
}

export {}
```
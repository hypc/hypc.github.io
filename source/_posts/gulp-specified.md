---
title: Task function must be specified
date: 2021-09-03 13:39:00
tags: [gulp]
---

> AssertionError [ERR_ASSERTION]: Task function must be specified

`gulpfile.js` 代码如下：

```js
gulp.task('default', ['build'], () => {});
```

执行抛出异常：

```
AssertionError [ERR_ASSERTION]: Task function must be specified
    at Gulp.set [as _setTask] (/path/to/project/node_modules/undertaker/lib/set-task.js:10:3)
    at Gulp.task (/path/to/project/node_modules/undertaker/lib/task.js:13:8)
    at Object.<anonymous> (/path/to/project/gulpfile.js:19:6)
    at Module._compile (internal/modules/cjs/loader.js:1072:14)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1101:10)
    at Module.load (internal/modules/cjs/loader.js:937:32)
    at Function.Module._load (internal/modules/cjs/loader.js:778:12)
    at Module.require (internal/modules/cjs/loader.js:961:19)
    at require (internal/modules/cjs/helpers.js:92:18)
    at requireOrImport (/path/to/project/node_modules/gulp-cli/lib/shared/require-or-import.js:19:11) {
  generatedMessage: false,
  code: 'ERR_ASSERTION',
  actual: false,
  expected: true,
  operator: '=='
}
```

经过查找，上述写法只能用于 `gulp4` 之前的版本，`gulp4` 之后改用以下写法：

```js
gulp.task('default', gulp.parallel('build'), () => {})  // 并行
// or
gulp.task('default', gulp.series('build'), () => {})    // 串行
```

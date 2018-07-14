---
title: ionic start错误
date: 2018-07-14 10:15:27
tags: [nodejs, ionic]
---

执行`ionic start my-app --v2`时出现以下错误：

```
Error with start undefined
Error Initializing app: There was an error with the spawned command: npminstall
There was an error with the spawned command: npminstall
```

这个错误是由于npm版本太旧造成的，执行以下命令：

```bash
npm install -g npm
```

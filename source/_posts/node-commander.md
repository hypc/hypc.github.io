---
title: 使用Nodejs编写命令行工具
date: 2019-01-17 16:31:20
tags: [nodejs]
---

## 编写命令行工具

### 创建一个项目

直接使用`npm init`初始化一个项目，`package.json`文件内容如下：

```json
{
  "name": "nb-cli",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### 创建可执行程序

创建文件`bin/nb-cli`，内容如下：

```javascript
#!/usr/bin/env node
'use strict';

console.log('cli');
console.log(process.argv);
```

<!--more-->

### 配置自定义命令

修改`package.json`文件，添加自定义命令：

```json
{
  "name": "nb-cli",
  "version": "1.0.0",
  "description": "",
  "bin": {
      "nb-cli": "./bin/nb-cli"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### 测试命令

使用`npm install -g`将模块安装全局模块中，然后使用`nb-cli`命令进行测试。

## 解析命令行参数

我们使用[Commander][]模块来解析参数。

### 安装Commander

```bash
npm install --save commander
```

### 解析参数

修改`bin/nb-cli`文件：

```javascript
#!/usr/bin/env node
'use strict';

var program = require('commander');

program
  .version('0.1.0')
  .option('-p, --peppers', 'Add peppers')
  .option('-P, --pineapple', 'Add pineapple')
  .option('-b, --bbq-sauce', 'Add bbq sauce')
  .option('-c, --cheese [type]', 'Add the specified type of cheese [marble]', 'marble')
  .parse(process.argv);

console.log('you ordered a pizza with:');
if (program.peppers) console.log('  - peppers');
if (program.pineapple) console.log('  - pineapple');
if (program.bbqSauce) console.log('  - bbq');
console.log('  - %s cheese', program.cheese);
```

### 测试命令

重新安装，然后使用`nb-cli`命令进行测试。

[Commander]: https://github.com/tj/commander.js

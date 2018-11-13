---
title: 【BUG】docker中安装node-sass失败解决办法
date: 2018-07-24 20:18:13
tags: [docker, npm, nodejs]
---

最近发现在使用`node:8.9-alpine`全局安装`node-sass@4.8`时，安装总是失败：

```bash
npm install -g node-sass@4.8 --sass-binary-site=https://npm.taobao.org/mirrors/node-sass/
```

执行结果如下：

```txt
/usr/local/bin/node-sass -> /usr/local/lib/node_modules/node-sass/bin/node-sass

> node-sass@4.8.3 install /usr/local/lib/node_modules/node-sass
> node scripts/install.js

Unable to save binary /usr/local/lib/node_modules/node-sass/vendor/linux_musl-x64-57 : { Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/node-sass/vendor'
    at Object.fs.mkdirSync (fs.js:885:18)
    at sync (/usr/local/lib/node_modules/node-sass/node_modules/mkdirp/index.js:71:13)
    at Function.sync (/usr/local/lib/node_modules/node-sass/node_modules/mkdirp/index.js:77:24)
    at checkAndDownloadBinary (/usr/local/lib/node_modules/node-sass/scripts/install.js:114:11)
    at Object.<anonymous> (/usr/local/lib/node_modules/node-sass/scripts/install.js:157:1)
    at Module._compile (module.js:643:30)
    at Object.Module._extensions..js (module.js:654:10)
    at Module.load (module.js:556:32)
    at tryModuleLoad (module.js:499:12)
    at Function.Module._load (module.js:491:3)
  errno: -13,
  code: 'EACCES',
  syscall: 'mkdir',
  path: '/usr/local/lib/node_modules/node-sass/vendor' }

> node-sass@4.8.3 postinstall /usr/local/lib/node_modules/node-sass
> node scripts/build.js
......
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@4.8.3 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@4.8.3 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-04-02T05_17_18_651Z-debug.log
```

<!--more-->

**解决办法是在安装时添加参数：`--unsafe-perm`**

```bash
npm install -g --unsafe-perm node-sass@4.8 --sass-binary-site=https://npm.taobao.org/mirrors/node-sass/
```

执行结果如下：

```txt
/usr/local/bin/node-sass -> /usr/local/lib/node_modules/node-sass/bin/node-sass

> node-sass@4.8.3 install /usr/local/lib/node_modules/node-sass
> node scripts/install.js

Downloading binary from https://npm.taobao.org/mirrors/node-sass//v4.8.3/linux_musl-x64-57_binding.node
Download complete  ] - :
Binary saved to /usr/local/lib/node_modules/node-sass/vendor/linux_musl-x64-57/binding.node
Caching binary to /root/.npm/node-sass/4.8.3/linux_musl-x64-57_binding.node

> node-sass@4.8.3 postinstall /usr/local/lib/node_modules/node-sass
> node scripts/build.js

Binary found at /usr/local/lib/node_modules/node-sass/vendor/linux_musl-x64-57/binding.node
Testing binary
Binary is fine
+ node-sass@4.8.3
added 187 packages in 15.667s
```

**错误详情如下**

```txt
/usr/local/bin/node-sass -> /usr/local/lib/node_modules/node-sass/bin/node-sass

> node-sass@4.8.3 install /usr/local/lib/node_modules/node-sass
> node scripts/install.js

Unable to save binary /usr/local/lib/node_modules/node-sass/vendor/linux_musl-x64-57 : { Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/node-sass/vendor'
    at Object.fs.mkdirSync (fs.js:885:18)
    at sync (/usr/local/lib/node_modules/node-sass/node_modules/mkdirp/index.js:71:13)
    at Function.sync (/usr/local/lib/node_modules/node-sass/node_modules/mkdirp/index.js:77:24)
    at checkAndDownloadBinary (/usr/local/lib/node_modules/node-sass/scripts/install.js:114:11)
    at Object.<anonymous> (/usr/local/lib/node_modules/node-sass/scripts/install.js:157:1)
    at Module._compile (module.js:643:30)
    at Object.Module._extensions..js (module.js:654:10)
    at Module.load (module.js:556:32)
    at tryModuleLoad (module.js:499:12)
    at Function.Module._load (module.js:491:3)
  errno: -13,
  code: 'EACCES',
  syscall: 'mkdir',
  path: '/usr/local/lib/node_modules/node-sass/vendor' }

> node-sass@4.8.3 postinstall /usr/local/lib/node_modules/node-sass
> node scripts/build.js

Building: /usr/local/bin/node /usr/local/lib/node_modules/node-sass/node_modules/node-gyp/bin/node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
gyp info it worked if it ends with ok
gyp verb cli [ '/usr/local/bin/node',
gyp verb cli   '/usr/local/lib/node_modules/node-sass/node_modules/node-gyp/bin/node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library=' ]
gyp info using node-gyp@3.6.2
gyp info using node@8.9.4 | linux | x64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` failed Error: not found: python2
gyp verb `which` failed     at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:152:21)
gyp verb `which` failed  python2 { Error: not found: python2
gyp verb `which` failed     at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:152:21)
gyp verb `which` failed   stack: 'Error: not found: python2\n    at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)\n    at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)\n    at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)\n    at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16\n    at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5\n    at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5\n    at FSReqWrap.oncomplete (fs.js:152:21)',
gyp verb `which` failed   code: 'ENOENT' }
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` failed Error: not found: python
gyp verb `which` failed     at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:152:21)
gyp verb `which` failed  python { Error: not found: python
gyp verb `which` failed     at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqWrap.oncomplete (fs.js:152:21)
gyp verb `which` failed   stack: 'Error: not found: python\n    at getNotFoundError (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:13:12)\n    at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:19)\n    at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)\n    at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16\n    at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5\n    at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5\n    at FSReqWrap.oncomplete (fs.js:152:21)',
gyp verb `which` failed   code: 'ENOENT' }
gyp ERR! configure error
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
gyp ERR! stack     at PythonFinder.failNoPython (/usr/local/lib/node_modules/node-sass/node_modules/node-gyp/lib/configure.js:483:19)
gyp ERR! stack     at PythonFinder.<anonymous> (/usr/local/lib/node_modules/node-sass/node_modules/node-gyp/lib/configure.js:397:16)
gyp ERR! stack     at F (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:68:16)
gyp ERR! stack     at E (/usr/local/lib/node_modules/node-sass/node_modules/which/which.js:80:29)
gyp ERR! stack     at /usr/local/lib/node_modules/node-sass/node_modules/which/which.js:89:16
gyp ERR! stack     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/index.js:42:5
gyp ERR! stack     at /usr/local/lib/node_modules/node-sass/node_modules/isexe/mode.js:8:5
gyp ERR! stack     at FSReqWrap.oncomplete (fs.js:152:21)
gyp ERR! System Linux 3.10.0-514.10.2.el7.x86_64
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/node-sass/node_modules/node-gyp/bin/node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd /usr/local/lib/node_modules/node-sass
gyp ERR! node -v v8.9.4
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok
Build failed with error code: 1
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! node-sass@4.8.3 postinstall: `node scripts/build.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the node-sass@4.8.3 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2018-04-02T05_17_18_651Z-debug.log
```

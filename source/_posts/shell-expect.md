---
title: expect命令
date: 2020-04-15 20:53:36
tags: [shell]
---

今天遇到一个需求，SSH远程登录之后自动切换到root用户，这里需要用到[expect][]命令，

[expect]: https://man.linuxde.net/expect-%e7%99%be%e7%a7%91%e7%af%87

```bash
#!/usr/bin/env expect

## 设置超时时间
set timeout 1
## 执行ssh命令，并等待 成功提示
spawn ssh -i ~/.ssh/id_rsa $username@1.2.3.4
expect "Login Success*"
## 执行切换用户命令，并等待 输入密码提示
send "sudo su -\r"
expect "Password:"
## 发送密码
send "$password\r"
## 保持交互状态，把控制权交给控制台，此时可继续手工操作
interact
```

<!--more-->

## 举一反三

这个需求的核心点在于，ssh登录之后，也就是切换shell环境之后执行一些其他命令。那么一些其他相似的需求也可以进行类似的处理：

* liunux安装软件包的时候，经常会有一些提示，需要人工确认才能继续进行，这时候可以采用except进行封装，
  让安装过程自动化（在批量安装的时候尤其有用）。当然，现在许多软件都有可选参数可跳过这些手动操作。
* 一些软件（如数据库、redis、rabbitmq等等）都提供一些命令行工具，而这些工具都有自己的环境，我们也可使用expect进行封装（当然很少有人这么干）。
* 一些工具或命令需要根据提示输入相应的内容才能继续运行，这时我们可以用expect使其自动化。
* 使用expect编写一些简单的监控、或自动化测试脚本。

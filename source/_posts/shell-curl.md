---
title: CURL命令
date: 2018-10-22 09:44:31
tags: [shell, curl]
---

curl命令是一个利用URL规则在命令行下工作的文件传输工具。
它支持文件的上传和下载，所以是综合传输工具，但按传统，习惯称curl为下载工具。
作为一款强力工具，curl支持包括HTTP、HTTPS、ftp等众多协议，还支持POST、
cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。
做网页处理流程和数据检索自动化，curl可以祝一臂之力。

## 常用选项

options                     |description
----------------------------|----------------------
-d/--data &lt;data&gt;      |传送数据
-H/--header &lt;line&gt;    |自定义头信息传递给服务器
-i/--include                |输出时包括protocol头信息
-I/--head                   |只显示请求头信息
-o/--output                 |把输出写到该文件中
-O/--remote-name            |把输出写到该文件中，保留远程文件的文件名
--progress                  |显示进度条
-s/--silent                 |静默模式，不输出任何东西
-w/--write-out [format]     |什么输出完成后
-X/--request &lt;command&gt;|指定什么命令

<!--more-->

## 下载文件

curl是将下载文件输出到stdout，将进度信息输出到stderr，不显示进度信息使用`--silent`选项。

```bash
curl URL --silent
# 将文件写入test.io文件，使用静默模式
curl http://man.linuxde.net/text.iso --silent -O
# 将文件写入filename.io文件，显示进度条
curl http://man.linuxde.net/test.iso -o filename.iso --progress
```

## 响应头

```bash
# 输出响应同时显示响应头
curl -i URL
# 只显示响应头信息
curl -I URL
```

## 其他HTTP请求

```bash
# 使用POST请求
curl -X POST URL
# 使用POST请求并传输数据
curl -X POST -d data URL
```

## 常用别名

```bash
alias curli='curl -i'
alias curlI='curl -I'
alias curlw='curl -i -w "\n\n----------------\ntime_connect: %{time_connect}\ntime_starttransfer: %{time_starttransfer}\ntime_total: %{time_total}\n"'
```

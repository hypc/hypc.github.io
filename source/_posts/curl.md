---
title: curl显示连接时间
date: 2018-07-05 17:14:21
tags: [shell]
---

```bash
alias curlw='curl -i -w "\n\n----------------\ntime_connect: %{time_connect}\ntime_starttransfer: %{time_starttransfer}\ntime_total: %{time_total}\n"'
```

- `time_namelookup`: DNS解析域名的时间
- `time_connect`: client和server端建立TCP连接的时间
- `time_starttransfer`: 从client发出请求；到web得到server响应第一个字节的时间
- `time_total`: client发出请求；到web得到server发送所有的相应数据的时间
- `speed_download`: 下载速度，单位byte/s

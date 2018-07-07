---
title: nginx强制文件下载
date: 2018-07-07 09:24:57
tags: [nginx]
---

```nginx
location ~ .*\.(gif|jpg|jpeg|bmp|png|mp3|wma|mp4|swf|txt)$ {
    if ($query_string ~ "download=true") {
        add_header Content-Disposition "attachment; filename=$request_filename";
    }
}
```

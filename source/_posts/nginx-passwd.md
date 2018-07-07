---
title: nginx设置密码访问
date: 2018-07-07 09:27:03
tags: [nginx]
---

## 安装apache的htpasswd工具

## 使用htpasswd工具生成密码文件

```bash
htpasswd -bc .htpasswd user userpasswd
```

## nginx配置

```nginx
location /xxx {
    root            /usr/local/example;
    index           index.html index.htm;
    auth_basic      "";             # 显示给用户的提示
    uth_basic_user_file    /etc/nginx/.htpasswd;  # 密码文件的位置
}
```

**注意**

上面配置可能造成php无法解析，此时需要在location里面再配置一层location：

```nginx
location /xxx {
    root            /usr/local/example;
    index           index.html index.htm;
    auth_basic      "";             # 显示给用户的提示
    uth_basic_user_file    /etc/nginx/.htpasswd;  # 密码文件的位置

    location ~ .*\.(php|php5)?$ {
        # ...
    }
}
```

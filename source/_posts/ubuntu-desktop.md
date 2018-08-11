---
title: Ubuntu手动添加启动图标
date: 2018-07-08 11:07:49
tags: [ubuntu]
---

在`/usr/share/applications`目录下创建一个以`.desktop`结尾的文件，里面内容如下：

```ini
[Desktop Entry]
Name=eclipse
Type=Application
Exec=/usr/share/eclipse/eclipse
Icon=/usr/share/eclipse/icon.xpm
Categories=Application;Utility;
Terminal=false
```

**关键字说明：**

- Name: 快捷方式的名字
- Type: 程序类型
- Exec: 要执行的程序
- Icon: 执行程序的图标
- Categories: 快捷方式显示的位置
- Terminal: 是否使用终端

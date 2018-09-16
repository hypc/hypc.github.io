---
title: Win7安装时转移用户文件夹
date: 2018-07-15 12:18:47
tags: [windows]
---

安装过程中，在要求输入用户名和密码时，先不输入任何信息，按`Shift+F10`呼出DOS窗口，
然后输入以下命令（假设用户文件夹转移到X盘，且X盘为NTFS分区）：

```bat
robocopy C:\Users X:\Users /E /COPYALL /XJ
rmdir C:\Users /S /Q
mklink /J C:\Users X:\Users
```

而后关闭DOS窗口，继续安装直至完成，如此安装的Win7，所有用户文件夹都在X盘上。

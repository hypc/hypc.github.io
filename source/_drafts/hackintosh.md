---
title: hackintosh
date: 2019-12-10 17:16:53
tags: [osx]
---

## 准备工作

* [原版镜像下载][download_url]
* 16G U盘
* [Clover EFI Bootloader][]
* [Clover Configurator][]

[download_url]: https://www.macxin.com/archives/10005.html
[Clover EFI Bootloader]: https://github.com/Dids/clover-builder/releases
[Clover Configurator]: https://mackie100projects.altervista.org/

## 制作安装U盘

### 格式化U盘

参考[文章](https://support.apple.com/zh-cn/HT208496)，
打开磁盘工具，将U盘格式化为`Mac OS 扩展（日志式）`。

### 制作安装盘

参考[文章](https://support.apple.com/zh-cn/HT201372)，使用`createinstallmedia`制作安装盘。

### 导入EFI引导文件到优盘

1. 下载最新版[Clover EFI Bootloader][]，将Clover安装到U盘。
2. 拷贝对应[机型](https://github.com/daliansky/Hackintosh)的驱动到U盘。

## 安装

1. 引导EFI进入安装界面
2. 选择`磁盘工具`，抹掉/格式化磁盘
3. 安装MacOS系统
4. 安装完成后，重启系统，然后将Clover引导复制到本地EFI分区中

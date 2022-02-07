---
title: CFG Lock
date: 2021-01-09 20:42:14
tags: [osx]
---

前两天重做了系统，今天开机一直卡在Logo画面，也不显示进度条。后来进BIOS，将`CFG Lock`修改为disabled便可以正常开机了。

`MSR 0xE2`：MSR全称是`Model Specific Register`，特定模块寄存器，属于非标准寄存器，用来控制CPU的工作环境和读取工作状态，比如电压，温度，功耗等非程序性能指标。
苹果系统的电源管理、CPU的`P-state`、`C-state`就是放在MSR寄存器里的。
大多数UEFI主板厂家，锁定了MSR寄存器的第15位为只读，也就是`MSR 0xE2 Locking`（BIOS 中叫`CFG Lock`）。
`MSR 0xE2`被锁定为只读后，AppleIntelCPUPowernamegement一旦去写入数据，马上就核心崩溃。
有些主板上有选项`CFG Lock`，其说明内容为`关闭或开启MSR 0xE2`，可以手动开关。

当你需要使用黑苹果时，则必须解锁`MSR 0xE2`，否则无法使用原生电源管理。关于如何解锁，网上有一堆资料，这里就不赘述了。

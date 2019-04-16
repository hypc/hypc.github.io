---
title: 搭建KMS服务器
date: 2019-04-15 14:29:34
tags: [kms, windows, office]
---

Key Management Service（简称:KMS），这个功能是在Windows Vista之后的产品中的一种新型产品激活机制。
我们可以利用手里闲置的VPS安装[vlmcsd][]来搭建KMS激活服务器，
这篇文章以CentOS为例，当然vlmcsd并不局限限于CentOS，如Ubuntu、Windows、MacOS等都可以安装服务端，原理和方法相同。

<!--more-->

## 安装vlmcsd服务

### 查看cpu架构信息

```bash
cat /proc/cpuinfo
```

```
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 42
model name	: Intel Xeon E312xx (Sandy Bridge)
stepping	: 1
microcode	: 0x1
cpu MHz		: 2099.998
cache size	: 4096 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx rdtscp lm constant_tsc rep_good nopl eagerfpu pni pclmulqdq ssse3 cx16 sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm fsgsbase smep erms xsaveopt
bogomips	: 4199.99
clflush size	: 64
cache_alignment	: 64
address sizes	: 46 bits physical, 48 bits virtual
power management:
...
```

可以看出我的机器是Intel 64位架构的机器。

### 下载vlmcsd

```bash
wget https://github.com/Wind4/vlmcsd/releases/download/svn1112/binaries.tar.gz
tar xvf binaries.tar.gz
cp binaries/Linux/intel/static/vlmcsd-x64-musl-static /usr/local/bin/vlmcsd
```

### 配置vlmcsd服务

```bash
tee /usr/lib/systemd/system/kms.service <<EOF
[Unit]
Description=vlmcsd - kms emulator
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/vlmcsd
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID

[Install]
WantedBy=multi-user.target
EOF
systemctl enable kms
systemctl start kms
```

## 激活Windows系统

这里使用系统内置命令`slmgr`激活系统，使用管理员身份运行CMD，然后执行下面命令：

```cmd
slmgr /skms <kms ip or host>
slmgr /ato
slmgr /xpr
```

**记住**，只能激活VL版Windows或Office，并且只有180天有效期，但只要服务还在运行，系统会自动续订，即可以认为是永久激活。

## 激活Office

使用管理员身份运行CMD，进入软件安装目录，然后执行下面命令：

```cmd
cscript ospp.vbs /sethst:<kms ip or host>
cscript ospp.vbs /act
cscript ospp.vbs /dstatus
```

**记住**，只能激活VL版Windows或Office，并且只有180天有效期，但只要服务还在运行，系统会自动续订，即可以认为是永久激活。

[vlmcsd]: https://github.com/Wind4/vlmcsd

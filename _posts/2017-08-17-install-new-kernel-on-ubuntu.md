---
title: AWS-EC2 ubuntu 16.04 安装升级内核
layout: post
date: '2017-08-17 16:24:58'
---

#### 检查当前内核

```
~$ uname -a
Linux ip-172-31-21-42 4.4.0-1022-aws #31-Ubuntu SMP Tue Jun 27 11:27:55 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

#### 下载预编译的新版内核

* [Ubuntu 官方内核 PPA](http://kernel.ubuntu.com/~kernel-ppa/mainline/)

> `sudo su` 切换到 root 用户做后续操作

```
~# wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9.44/linux-headers-4.9.44-040944_4.9.44-040944.201708161731_all.deb
~# wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9.44/linux-headers-4.9.44-040944-generic_4.9.44-040944.201708161731_amd64.deb
~# wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.9.44/linux-image-4.9.44-040944-generic_4.9.44-040944.201708161731_amd64.deb
```

#### 安装内核

```
~# dpkg -i linux-*.deb
```

#### 卸载旧版内核

查看当前已安装内核
```
~# dpkg -l | grep linux-image
ii  linux-image-4.4.0-1022-aws          4.4.0-1022.31                              amd64        Linux kernel image for version 4.4.0 on 64 bit x86 SMP
ii  linux-image-4.4.0-1031-aws          4.4.0-1031.40                              amd64        Linux kernel image for version 4.4.0 on 64 bit x86 SMP
ii  linux-image-4.9.44-040944-generic   4.9.44-040944.201708161731                 amd64        Linux kernel image for version 4.9.44 on 64 bit x86 SMP
ii  linux-image-aws                     4.4.0.1031.33                              amd64        Linux kernel image for Amazon Web Services (AWS) systems.
```

卸载
```
~# apt remove linux-image-4.4.0-*
```

更新 grub
```
~# update-grub
```

#### 确认当前内核

```
~# ls /boot/vmlinuz*
/boot/vmlinuz-4.9.44-040944-generic
```

#### 重启系统

```
~# reboot
```

#### 开启 BBR

* [GetNAS](https://www.getnas.com/2017/07/2155.html)
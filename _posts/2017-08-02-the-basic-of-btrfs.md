---
title: Btrfs 文件系统基础
layout: post
date: '2017-08-02 13:26:31'
---

列出所有 btrfs 文件系统：
```
~$ sudo btrfs filesystem show

Label: none  uuid: d510f167-1d2f-42bf-a9bd-485c4aca42f2
	Total devices 1 FS bytes used 907.86MiB
	devid    1 size 7.00GiB used 2.13GiB path /dev/sda1
```

查看文件系统基本信息：
```
~$ sudo btrfs filesystem show /dev/sda1

Label: none  uuid: d510f167-1d2f-42bf-a9bd-485c4aca42f2
	Total devices 1 FS bytes used 908.03MiB
	devid    1 size 7.00GiB used 2.13GiB path /dev/sda1
```

查看挂载点详情：
```
~$ df -T

文件系统         类型       1K-块   已用    可用   已用% 挂载点
udev           devtmpfs  499096      0  499096    0% /dev
tmpfs          tmpfs     102048   1696  100352    2% /run
/dev/sda1      btrfs    7339008 977876 5689964   15% /
```

```
~$ sudo btrfs filesystem df /

Data, single: total=1.41GiB, used=876.77MiB
System, DUP: total=8.00MiB, used=16.00KiB
Metadata, DUP: total=358.31MiB, used=31.08MiB
GlobalReserve, single: total=16.00MiB, used=0.00B
```
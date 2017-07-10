---
title: 使用 DD 命令将 ISO 镜像写入 U 盘
layout: post
date: '2017-07-10 21:55:02'
---

## For MacOS

磁盘实用工具抹掉 U 盘。

列出 U 盘
```
$ diskutil list

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *8.0 GB     disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:       Microsoft Basic Data                         7.8 GB     disk2s2
```

卸载 U 盘
```
$ diskutil unmountDisk /dev/disk2
Unmount of all volumes on disk2 was successful
```

写镜像
```
$ sudo dd if=ubuntu-16.04.2-desktop-amd64.iso of=/dev/rdisk2 bs=1m
Password:
1482+1 records in
1482+1 records out
1554186240 bytes transferred in 327.064864 secs (4751921 bytes/sec)
```
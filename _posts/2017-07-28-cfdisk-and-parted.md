---
title: Linux 命令行下硬盘分区、格式化
layout: post
date: '2017-07-28 11:06:29'
---

fdisk 是 Linux 系统环境下最知名的磁盘分区工具，但它不支持 GPT 分区表，因此新版系统已经将其淘汰。对于 Debian 取而代之的是 `parted`，在 Gnome 环境下的 Gparted 是它的窗口版。

> `cfdisk` 是命令行下的伪图形化分区工具，也非常好用。

#### 创建分区

安装

```
herald@homeserver:~$ sudo apt install parted
```

查看所有磁盘及分区情况：

```
herald@homeserver:~$ sudo parted -l
```

交互式的磁盘分区操作

```
herald@homeserver:~$ sudo parted /dev/sdd
```


在使用 parted 工具交互式创建磁盘分区时，`start` 和 `end` 表示硬盘的起止位置，使用数字，单位为 `MB`：

* start = 0，end = -1，该分区使用整个硬盘空间
* start = 0，end = 100000，该分区从硬盘起始位置开始占用 100GB 空间

#### 格式化分区

分区格式化使用 `mkfs` 工具，它还有几个方便的子程序 mkfs.bfs，mkfs.cramfs，mkfs.ext2，mkfs.ext3，mkfs.ext4，mkfs.minix。

将分区 `/dev/sdd1` 格式化为 `ext4` 格式：

```
herald@homeserver:~$ sudo mkfs -t ext4 /dev/sdd1
```

或

```
herald@homeserver:~$ sudo mkfs.ext4 /dev/sdd1
```

格式化实例

```
herald@homeserver:~$ sudo mkfs -t ext4 /dev/sdd2
mke2fs 1.43.4 (31-Jan-2017)
创建含有 24414208 个块（每块 4k）和 6111232 个inode的文件系统
文件系统UUID：67c069d6-f846-4f31-b76e-6526cdb9c957
超级块的备份存储于下列块：
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872

正在分配组表： 完成
正在写入inode表： 完成
创建日志（131072 个块）完成
写入超级块和文件系统账户统计信息： 已完成
```
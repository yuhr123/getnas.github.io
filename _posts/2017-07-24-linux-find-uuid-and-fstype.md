---
title: Linux 查看 UUID 和文件系统类型
layout: post
date: '2017-07-24 16:29:03'
---

UUID 是唯一不会变化的设备识别代码，比 `/dev/sdX` 类似的设备名称可靠。

#### 查看设备 UUID

方法一

```
$ ls -l /dev/disk/by-uuid/

总用量 0
lrwxrwxrwx 1 root root 11 7月  24 16:26 007141b8-720d-460d-bc99-63a2cf7b8afd -> ../../md127
lrwxrwxrwx 1 root root 10 7月  24 16:26 3ad083a0-4fff-42d6-a975-81dc527df91f -> ../../sdc3
lrwxrwxrwx 1 root root 10 7月  24 16:26 50f21cc8-eb3e-4490-989e-307233b2e3bb -> ../../sdc2
lrwxrwxrwx 1 root root 10 7月  24 16:26 9471-186B -> ../../sdc1
```

方法二

```
herald@homeserver:~$ sudo blkid

/dev/sda1: UUID="2e74fab8-3e14-80ea-2090-584eae3572bb" UUID_SUB="04ebddc1-38c8-cb74-647a-af4009d486eb" LABEL="any:mydata" TYPE="linux_raid_member" PARTLABEL="primary" PARTUUID="f68739c2-38fd-42da-a5c0-ee676e60ddee"
/dev/sdb1: UUID="2e74fab8-3e14-80ea-2090-584eae3572bb" UUID_SUB="af9b4df8-7456-0eb6-02db-de02d3f538ae" LABEL="any:mydata" TYPE="linux_raid_member" PARTUUID="81bba0c1-01"
/dev/md127: UUID="007141b8-720d-460d-bc99-63a2cf7b8afd" UUID_SUB="75ffc230-8f78-45f2-a7f3-173983bd3e28" TYPE="btrfs"
/dev/sdc1: UUID="9471-186B" TYPE="vfat" PARTUUID="e5938927-4aef-4329-af99-a05df7e4761f"
/dev/sdc2: UUID="50f21cc8-eb3e-4490-989e-307233b2e3bb" TYPE="ext4" PARTUUID="9ed4fbb7-5829-45e3-915c-8896c0b93337"
/dev/sdc3: UUID="3ad083a0-4fff-42d6-a975-81dc527df91f" TYPE="swap" PARTUUID="9355553e-651f-4647-958a-161dd242305f"
```

#### 查看文件系统类型

```
$ df -T

文件系统       类型         1K-块     已用      可用 已用% 挂载点
udev           devtmpfs    963416        0    963416    0% /dev
tmpfs          tmpfs       194968     3548    191420    2% /run
/dev/sdc2      ext4      12663292  1333564  10666740   12% /
tmpfs          tmpfs       974824        0    974824    0% /dev/shm
tmpfs          tmpfs         5120        0      5120    0% /run/lock
tmpfs          tmpfs       974824        0    974824    0% /sys/fs/cgroup
/dev/sdc1      vfat        523248      132    523116    1% /boot/efi
/dev/md127     btrfs    488385344 63385964 423112340   14% /home/herald/mydata
tmpfs          tmpfs       194964        0    194964    0% /run/user/1000
```
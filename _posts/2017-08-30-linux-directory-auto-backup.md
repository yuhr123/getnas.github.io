---
title: Linux 目录自动备份脚本
layout: post
date: '2017-08-30 20:00:38'
---

```
#!/bin/bash
#Purpose = Backup of Important Data
#Created on 2017年08月30日
#Author = Herad Yu
#Version 1.0
#START
# 备份时间
TIME=`date +"%Y%m%d"`
# 备份文件名称
FILENAME=backup-$TIME.tar.gz
# 源目录（要备份的目录）
SRCDIR=/var/www/bavaria-china
# 目标目录（备份存储的位置）
DESDIR=/home/debian/cdisk/backup
tar -cpzf $DESDIR/$FILENAME $SRCDIR

#END
```
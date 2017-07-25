---
title: Samba 的 rlimit 设置
layout: post
date: '2017-07-25 19:57:10'
---

在用 `testparm` 检测 samba 配置文件时，会有 rlimit_max 的设置提示。

```
herald@homeserver:~$ testparm

Load smb config files from /etc/samba/smb.conf
rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
```

解决方法是修改 /etc/security/limits.conf 文件，在其中添加一行：

```
herald@homeserver:~$ sudo nano /etc/security/limits.conf

...
*               -       nofile          16384
# End of file
...
```
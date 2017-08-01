---
title: Samba 利用 browseable 实现按身份显示共享
layout: post
date: '2017-08-01 13:26:31'
---

Samba 的 `browseable` 选项用来设置共享是否可以被访问，结合 `include` 选项引用外部配置文件，搭配 samba 变量，即可实现不同用户登录显示不同的共享。

下例为 `tmp` 共享仅对 `herald` 用户可见，即只有该用户登录才能看到此共享。

创建 `/etc/samba/smb.conf.herald` 文件并添加配置：
```
 browseable = yes
```


Samba 主配置文件 `/etc/samba/smb.conf` 中添加 `include` 选项：
```
...
[tmp]
  comment = tmp directory
  read only = no
  locking = no
  browseable = no
  path = /home/herald/mydata/tmp
  guest ok = no
  valid users = herald
  include = /etc/samba/smb.conf.%U
...
```

其中的 `%U` 变量代表访问共享的用户名，[查看更多 Samba 可用的变量](https://www.samba.org/samba/docs/using_samba/ch06.html#samba2-CHP-6-TABLE-2)。
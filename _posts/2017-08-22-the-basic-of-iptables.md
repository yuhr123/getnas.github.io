---
title: iptables 基本用法
layout: post
date: '2017-08-22 11:05:36'
---

以 CentOS 系统为例
```
[herald@centos /]$ cat /etc/centos-release
CentOS Linux release 7.3.1611 (Core)
```

检查是否已经安装 iptables 
```
[herald@centos ~]$ rpm -q iptables
iptables-1.4.21-17.el7.x86_64
```

查看 iptables 是否在运行
```
[herald@centos ~]$ lsmod | grep ip_tables
ip_tables  27115  5 iptable_security,iptable_filter,iptable_mangle,iptable_nat,iptable_raw
```

列出当前所有 iptables 规则
```
[herald@centos ~]$ sudo iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
ACCEPT     all  --  anywhere             anywhere
.......
Chain IN_public_allow (1 references)
target    prot    opt source               destination
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:ssh ctstate NEW
```

清空当前所有规则
```
[root@centos herald]# iptables -L
```

允许 SSH 连接
```
[root@centos herald]# iptables -A INPUT -p tcp --dport 22 -j ACCEPT
[root@centos herald]# iptables -L
Chain INPUT (policy ACCEPT)
target    prot     opt     source               destination
ACCEPT     tcp        --     anywhere            anywhere             tcp dpt:ssh
```
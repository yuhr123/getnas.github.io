---
title: 小米路由器3 设置 dnsmasq
layout: post
date: '2017-08-16 10:22:15'
---

创建自定义配置文件 `/etc/dnsmasq.d/dnsmasq.custom.conf`

添加记录

```
address=/homeserver.local/192.168.10.103
```

上游 DNS 服务器在路由器管理界面网络连接部分设置
---
title: qBittorrent 开机启动 for Debian 9
layout: post
date: '2017-07-25 23:29:43'
---

安装 qBittorrent-nox ，这个版本没有 GUI，主要在服务器使用。
```
herald@homeserver:~$ sudo apt install qbittorrent-nox
```

创建 systemd 系统级配置文件
```
herald@homeserver:~$ sudo nano /lib/systemd/system/qbittorrent.service
```

配置内容如下，替换 `qbtuser` 为实际用户，`8080` 端口也可以替换：
```
[Unit]
Description=qBittorrent Daemon Service
After=network.target

[Service]
User=qbtuser
ExecStart=/usr/bin/qbittorrent-nox --webui-port=8080

[Install]
WantedBy=multi-user.target

```

设置开机启动

```
herald@homeserver:~$ sudo systemctl enable qbittorrent.service
```

远程访问的初始用户名 `admin` 密码 `adminadmin`。
---
title: Aria2 以开机启动 for systemd
layout: post
date: '2017-07-26 09:17:36'
---

创建配置文件目录

```
herald@homeserver:~$ mkdir -p ~/.config/aria2
```

创建配置文件

```
herald@homeserver:~$ nano ~/.config/aria2/aria2.conf
```

按需设置配置文件：

```
dir=/home/herald/mydata/downloads
max-connection-per-server=5
```

创建系统级别 systemd 配置文件

```
herald@homeserver:~$ sudo nano /lib/systemd/system/aira2.service
```

粘贴以下内容，按需调整 `ExecStart` 部分的命令和参数：

```
[Unit]
Description=Aria2c download manager
After=network.target

[Service]
Type=simple
User=herald
ExecStart=/usr/bin/aria2c --console-log-level=warn --enable-rpc --rpc-listen-all --conf-path=/home/herald/.config/aria2/aria2.conf

[Install]
WantedBy=multi-user.target
```

设置开机启动

```
herald@homeserver:~$ sudo systemctl enable aria2.service
```
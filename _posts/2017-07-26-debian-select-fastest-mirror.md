---
title: netselect-apt 选择最快的 Debian 软件源
layout: post
date: '2017-07-26 09:17:36'
---

安装

```
herald@homeserver:~$ sudo apt install netslect-apt
```

标准用法

```
herald@homeserver:~$ sudo netselect-apt
```

程序会自动检测全球几百个 Debian 软件源，并找到访问速度最快的那一个，随后在当前目录下生成 `sources.list` 文件，复制到 `/etc/apt` 目录替换即可。

```
herald@homeserver:~$ sudo cp ./sources.list /etc/apt/sources.list
```
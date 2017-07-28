---
title: Debian 9 编译安装 netatalk 3.1.11
layout: post
date: '2017-07-28 11:06:29'
---

#### 安装依赖

```
$ sudo apt install build-essential libevent-dev libssl-dev libgcrypt-dev libkrb5-dev libpam0g-dev libwrap0-dev libdb-dev libtdb-dev libmariadbclient-dev avahi-daemon libavahi-client-dev libacl1-dev libldap2-dev libcrack2-dev systemtap-sdt-dev libdbus-1-dev libdbus-glib-1-dev libglib2.0-dev libio-socket-inet6-perl tracker libtracker-sparql-1.0-dev libtracker-miner-1.0-dev
```

> 如果 tracker 版本不正确，使用 "apt search libtracker" 查看正确的版本号。


参考：[http://netatalk.sourceforge.net/wiki/index.php/Install_Netatalk_3.1.11_on_Debian_9_Stretch](http://netatalk.sourceforge.net/wiki/index.php/Install_Netatalk_3.1.11_on_Debian_9_Stretch)

#### 下载源码包

官方仓库：[https://sourceforge.net/projects/netatalk/files/netatalk/3.1.11/](https://sourceforge.net/projects/netatalk/files/netatalk/3.1.11/)

#### 编译安装

`--with-init-style=debian-systemd`  根据实际的系统和启动器类型选择，`--with-tracker-pkgconfig-version=1.0` 版本号必须对应系统安装的 tracker 实际版本。

查看 tracker 版本号：

```
$ pkg-config --list-all | grep tracker
tracker-miner-1.0     tracker-miner - A library to develop tracker data miners
tracker-sparql-1.0    tracker-sparql - Tracker : A library to perform SPARQL queries and updates in the              Tracker Store
```

编译

```
$ ./configure \
        --with-init-style=debian-systemd \
        --without-libevent \
        --without-tdb \
        --with-cracklib \
        --enable-krbV-uam \
        --with-pam-confdir=/etc/pam.d \
        --with-dbus-daemon=/usr/bin/dbus-daemon \
        --with-dbus-sysconf-dir=/etc/dbus-1/system.d \
        --with-tracker-pkgconfig-version=1.0
```

安装

```
$ make
# make install
```

编辑配置文件

```
$ sudo nano /usr/local/etc/afp.conf

;
; Netatalk 3.x configuration file
;

[Global]
; Global server settings

; [Homes]
;basedir regex = /home/herald/tm

;[My AFP Volume]
;path = /home/herald/tm
;valid users = herald

[My Time Machine Volume]
path = /home/herald/tm/timemachine
time machine = yes
valid users = herald
```

#### 启动和开机启动

```
# systemctl enable avahi-daemon
# systemctl enable netatalk
# systemctl start avahi-daemon
# systemctl start netatalk
```

#### 排错

如果 Mac OS X 上无法发现存储设备，可能是因为系统默认不显示非官方支持的网络磁盘，使用以下命令开启：

```
defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1
```
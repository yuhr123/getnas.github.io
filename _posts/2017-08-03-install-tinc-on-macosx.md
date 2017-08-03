---
title: Mac OS X 安装 Tinc
layout: post
date: '2017-08-03 13:26:31'
---

```
~$ brew install tinc
~$ brew cask install tuntap
```

#### 文件位置
* 二进制文件：/usr/local/sbin/tincd
* 配置文件：/usr/local/etc/tinc

创建配置文件目录结构：
```
~$ sudo mkdir -p /usr/local/etc/tinc/p2pvpn
```

创建 `tinc.con` 配置文件：
```
~$ sudo nano /usr/local/etc/tinc/tinc.conf

Name = macbook
Device = /dev/tap0
ConnectTo = vultr
```

创建 `macbook` 主机配置文件：
```
~$ sudo nano /usr/local/etc/tinc/hosts/macbook

Subnet = 10.0.0.4/32
```

创建 `tinc-up`：
```
~$ sudo nano /usr/local/etc/tinc/tinc-up

#!/bin/sh
ifconfig $INTERFACE 10.0.0.4 netmask 255.255.255.0
```

创建 `tinc-down`：
```
~$ sudo nano /usr/local/etc/tinc/tinc-down

#!/bin/sh
ifconfig $INTERFACE down
```

赋予 tinc- 脚本可执行权限：
```
~$ sudo chmod -v +x /usr/local/etc/tinc/tinc-{up,down}
```

生成密钥对：
```
~$ sudo /usr/local/sbin/tincd -n p2pvpn -K4096

Generating 4096 bits keys:
...................................................................................++ p
...............................................................................................................++ q
Done.
Please enter a file to save private RSA key to [/usr/local/etc/tinc/p2pvpn/rsa_key.priv]:
Please enter a file to save public RSA key to [/usr/local/etc/tinc/p2pvpn/hosts/macbook]:
```

与 Server 端互换位于 `hosts` 目录的主机配置文件

启动测试：
```
~$ sudo /usr/local/sbin/tincd -n p2pvpn --pidfile=/var/run/tincd.pid
```

### Auto start

创建配置文件 `/Library/LaunchDaemons/p2pvpn.tinc.plist`

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>KeepAlive</key>
    <true/>
    <key>Label</key>
    <string>tinc.p2pvpn</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/sbin/tincd</string>
        <string>-n</string>
        <string>p2pvpn</string>
        <string>-D</string>
				<string>--pidfile=/var/run/tincd.pid</string>
    </array>
</dict>
</plist>
```

启动服务：
```
~$ sudo launchctl load -w /Library/LaunchDaemons/p2pvpn.tinc.plist
```

禁用服务：
```
~$ sudo launchctl unload -w /Library/LaunchDaemons/p2pvpn.tinc.plist
```

#### See also
* 参考一：https://blog.swineson.me/tinc-tap-vpn-on-macos-with-dhcp/
* 参考二：https://chadderstech.wordpress.com/2015/09/08/tinc-on-mac-os-x/
* 参考三：https://www.tinc-vpn.org/examples/osx-install/#index5h3
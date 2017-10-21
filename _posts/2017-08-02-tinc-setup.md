---
title: tinc 配置
layout: post
date: '2017-08-02 13:26:31'
---

### 安装
```
~$ sudo apt install tinc

如果没有 ifconfig 命令：
~$ sudo apt install net-tools
```

### Server

创建配置文件目录：
```
~$ sudo mkdir -p /etc/tinc/p2pvpn/hosts
```

创建 `tinc.conf` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc.conf

Name = vultr
Device = /dev/net/tun
```

创建 `vultr` 配置文件（Address 为公网 IP）：
```
~$ sudo nano /etc/tinc/p2pvpn/hosts/vultr

Address = 45.63.45.63
Subnet = 10.0.0.1/32
```

创建 `tinc-up` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc-up

对于 ifconfig
#!/bin/sh
ifconfig $INTERFACE 10.0.0.1 netmask 255.255.255.0

对于 ip
#!/bin/sh
ip link set $INTERFACE up
ip addr add  10.0.0.1/24 dev $INTERFACE
```

创建 `tinc-down` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc-down

对于 ifconfig
#!/bin/sh
ifconfig $INTERFACE down

对于 ip
#!/bin/sh
ip addr del 10.0.0.1/24 dev $INTERFACE
ip link set $INTERFACE down
```

为脚本设置可执行权限：
```
~$ sudo chmod -v +x tinc-{up,down}
```

生成密钥对（公钥会自动追加到 `hosts/vultr` 文件尾部）：
```
~$ sudo tincd -n p2pvpn -K4096
```

将 `vultr` 配置文件复制到 `Client` 的 `/etc/tinc/p2pvpn/hosts` 目录。

启动：
```
~$ sudo tincd -n p2pvpn
```

### Client

创建配置文件目录：
```
~$ sudo mkdir -p /etc/tinc/p2pvpn/hosts
```

创建 `tinc.conf` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc.conf

Name = homeserver
Device = /dev/net/tun
ConnectTo = vultr
```

创建 `homeserver` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/hosts/homeserver

Subnet = 10.0.0.2/32
```

创建 `tinc-up` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc-up

#!/bin/sh
ifconfig $INTERFACE 10.0.0.2 netmask 255.255.255.0

对于 ip
#!/bin/sh
ip link set $INTERFACE up
ip addr add  10.0.0.2/24 dev $INTERFACE
```

创建 `tinc-down` 配置文件：
```
~$ sudo nano /etc/tinc/p2pvpn/tinc-down

#!/bin/sh
ifconfig $INTERFACE down

对于 ip
#!/bin/sh
ip addr del 10.0.0.2/24 dev $INTERFACE
ip link set $INTERFACE down
```

为脚本设置可执行权限：
```
~$ sudo chmod -v +x tinc-{up,down}
```

生成密钥对（公钥会自动追加到 `hosts/homeserver` 文件尾部）：
```
~$ sudo tincd -n p2pvpn -K4096
```

将 `homeserver` 配置文件复制到 `Server` 的 `/etc/tinc/p2pvpn/hosts` 目录。

启动：
```
~$ sudo tincd -n p2pvpn
```

### Mutiple Clinet

可参照 `Clinet` 添加多个客户端，客户端只需与服务端交换带公钥的主机名配置文件即可，不需要和其他的客户端交换。

### Mac OS X 客户端

使用 brew 安装，程序目录位于 `/usr/local/Cellar/tinc`
```
~$ brew install tinc
```

创建配置文件目录
```
~$ mkdir -p /usr/local/etc/tinc
```

其他配置方式与前面记录的方法相同。

### Windows 客户端

在程序安装目录创建` p2pvpn` 目录及 `hosts` 子目录，tinc.conf 配置文件，注意，要用 Interface 替换 Device：
```
Name = lenovo
Interface = VPN
ConnectTo = vultr
```

无需创建 tinc-up 和 tinc-down 配置文件，而是要以管理员身份打开命令行安装虚拟网卡，程序安装目录中提供了 32 位和 64 位两个版本，根据所使用的操作系统选择，执行名录中的 `addtap.bat`。 

虚拟网卡安装完成后，重命名为 `VPN`，此处的名称与 tinc.conf 配置文件中 Interface 的名称相对应。

直接在虚拟网卡属性中配置 IPv4 地址和掩码。

在命令行中启动服务 `tincd.exe -n p2pvpn` 后会自动创建系统服务，配置无误服务即可正常启动。

### Auto start

```
~$ sudo systemctl enable tinc.service

~$ sudo systemctl enable tinc@p2pvpn

~$ sudo systemctl enable tinc@another_name
```

### 重点备忘

无论客户端还是服务端，主机文件中的 `Subnet` 中的 IP 部分要与 `tinc-up` 中的 IP 地址相一致，否则会出现节点之间 PING 不通的现象。

#### See also

* 官网：[https://www.tinc-vpn.org](https://www.tinc-vpn.org)
* 参考一：[http://xmodulo.com/how-to-install-and-configure-tinc-vpn.html](http://xmodulo.com/how-to-install-and-configure-tinc-vpn.html)
* 参考二：[https://smartystreets.com/blog/2015/10/how-to-setup-a-tinc-vpn](https://smartystreets.com/blog/2015/10/how-to-setup-a-tinc-vpn)
* 参考三：[https://wiki.archlinux.org/index.php/Tinc](https://wiki.archlinux.org/index.php/Tinc)
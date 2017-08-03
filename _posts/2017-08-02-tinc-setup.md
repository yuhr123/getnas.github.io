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

### Auto start

```
~$ sudo systemctl enable tinc.service

~$ sudo systemctl enable tinc@p2pvpn

~$ sudo systemctl enable tinc@another_name
```

#### See also

* 官网：[https://www.tinc-vpn.org](https://www.tinc-vpn.org)
* 参考一：[http://xmodulo.com/how-to-install-and-configure-tinc-vpn.html](http://xmodulo.com/how-to-install-and-configure-tinc-vpn.html)
* 参考二：[https://smartystreets.com/blog/2015/10/how-to-setup-a-tinc-vpn](https://smartystreets.com/blog/2015/10/how-to-setup-a-tinc-vpn)
* 参考三：[https://wiki.archlinux.org/index.php/Tinc](https://wiki.archlinux.org/index.php/Tinc)
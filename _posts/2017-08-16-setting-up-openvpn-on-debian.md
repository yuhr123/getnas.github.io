---
title: Debian 安装 OpenVPN
layout: post
date: '2017-08-16 11:46:46'
---

### 安装

```
~$ sudo apt install openvpn
```

复制 `openvpn` 目录到 `/etc`
```
~$ sudo cp -r /usr/share/doc/openvpn /etc/
```

复制 `easy-rsa` 目录到 `/etc/openvpn`
```
~$ sudo cp -r /usr/share/easy-rsa /etc/openvpn/
```


### 配置 CA

编辑 `vars` 文件，确保 `KEY_COUNTRY`，`KEY_PROVINCE`，`KEY_CITY`，`KEY_ORG`，`KEY_EMAIL` 这几项必须填写，`KEY_CONFIG` 根据当前安装的 openssl 版本设置
```
/etc/openvpn/easy-rsa$ sudo nano vars
```

>  export KEY_CONFIG=$EASY_RSA/openssl-1.0.0.cnf

初始化 PKI
```
/etc/openvpn/easy-rsa# . ./vars
/etc/openvpn/easy-rsa# ./clean-all
/etc/openvpn/easy-rsa# ./build-ca
```

### 生成服务器端证书

```
/etc/openvpn/easy-rsa# ./build-key-server server
```

为服务器端生成 Diffie Hellman
```
/etc/openvpn/easy-rsa# ./build-dh
```

### 生成客户端证书

```
/etc/openvpn/easy-rsa# ./build-key client1
```

### 服务器配置文件

解压服务器配置文件
```
/etc/openvpn/examples/sample-config-files# gunzip -c server.conf.gz > server.conf
```

### 参考

* * [https://openvpn.net/index.php/open-source/documentation/howto.html](https://openvpn.net/index.php/open-source/documentation/howto.html)
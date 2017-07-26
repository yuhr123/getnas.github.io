---
title: 小米路由器 3 安装 opkg
layout: post
date: '2017-07-26 09:17:36'
---

小米路由器开启 SSH 方法：https://d.miwifi.com/rom/ssh

#### ssh key 登录

复制 public key 到路由器：

```
~$ scp ~/.ssh/id_rsa.pub root@192.168.31.1:/etc/dropbear/authorized_keys
```

设置文件权限：

```
~# chmod 600 /etc/dropbear/authorized_keys
```

#### 安装 opkg

1、从任意 mt7620 芯片的 openwrt 固件包中提取 `opkg`，复制到小米路由的 `/data` 路径下。

2、替换 `/etc/opkg.conf` 文件内容：

```
src/gz attitude_adjustment_base http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/base
src/gz attitude_adjustment_packages http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/packages/
src/gz attitude_adjustment_luci http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/luci/
src/gz attitude_adjustment_management http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/management/
src/gz attitude_adjustment_oldpackages http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/oldpackages/
src/gz attitude_adjustment_routing http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/routing/
src/gz openwrt_dist http://openwrt-dist.sourceforge.net/releases/ramips/packages
src/gz openwrt_dist_luci http://openwrt-dist.sourceforge.net/releases/luci/packages
dest root /data
dest ram /tmp
lists_dir ext /data/var/opkg-lists
option overlay_root /data
arch all 100
arch ramips 200
arch ramips_24kec 300
```

3、修改/etc/profile文件，找到export PATH=一行修改为：

```
export PATH=/bin:/sbin:/usr/bin:/usr/sbin:/data/usr/sbin:/data/usr/bin
```

再这一行下面新添加一行：

```
export LD_LIBRARY_PATH=/data/usr/lib
```

4、使环境变量即时生效：

```
export PATH=$PATH:/data/usr/bin:/data/usr/sbin
export LD_LIBRARY_PATH=LD_LIBRARY_PATH:/data/usr/lib
```

5、更新 opkg

```
opkg update
```

6、提示错误，缺少 `libc*` 的解决办法：

```
cd /data
wget http://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/base/libc_0.9.33.2-1_ramips_24kec.ipk 
opkg install libc_*.ipk
```


> 如果链接失效可能是更新了包，可以到 `https://downloads.openwrt.org/barrier_breaker/14.07/ramips/mt7620a/packages/base/` 搜索 `libc_` 找到最新下载地址。

参考：http://www.ywlib.com/archives/102.html


#### 附：VI 的行删除命令

|命令|解释|
| - | - |
| x      |      删除当前光标下的字符 |
| dw  |     删除光标之后的单词剩余部分 |
| d$    |   删除光标之后的该行剩余部分 |
| dd   |    删除当前行 |
| c |        功能和d相同，区别在于完成删除操作后进入INSERT MODE |
| cc  |    也是删除当前行，然后进入INSERT MODE |
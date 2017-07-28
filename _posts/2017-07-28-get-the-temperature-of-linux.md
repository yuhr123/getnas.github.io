---
title: Linux 下查看 CPU 和硬盘温度
layout: post
date: '2017-07-28 11:06:29'
---

`lm-sensors` 用于探测 CPU 温度，`hddtemp` 用于探测硬盘温度。

安装

```
herald@homeserver:~$ sudo apt install lm-sensors hddtemp
```

查看 CPU 温度

```
herald@homeserver:~$ sensors

acpitz-virtual-0
Adapter: Virtual device
temp1:        +26.8°C  (crit = +90.0°C)

coretemp-isa-0000
Adapter: ISA adapter
Core 0:       +51.0°C  (high = +105.0°C, crit = +105.0°C)
Core 1:       +51.0°C  (high = +105.0°C, crit = +105.0°C)
Core 2:       +51.0°C  (high = +105.0°C, crit = +105.0°C)
Core 3:       +51.0°C  (high = +105.0°C, crit = +105.0°C)
```

查看硬盘 `/dev/sda` 的温度

```
herald@homeserver:~$ sudo hddtemp /dev/sda

/dev/sda: ST500LT012-1DG142: 44°C
```
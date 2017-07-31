---
title: Debian 9 切换默认 Python 版本
layout: post
date: '2017-07-31 20:53:54'
---

Debian 9 同时预装 python 2 和 python 3，可以使用 `update-alternavtives` 实现默认版本的切换。

**列出系统预装的 python 版本：**

```
~$ ls /usr/bin/python*
/usr/bin/python  /usr/bin/python2  /usr/bin/python2.7  /usr/bin/python3  /usr/bin/python3.5  /usr/bin/python3.5m  /usr/bin/python3m
```

**查看 python 是否创建了符号链接：**

```
~$ sudo update-alternatives --list python
update-alternatives: error: no alternatives for python
```

**对各版本 python 创建符号链接：**

```
~$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
update-alternatives: 使用 /usr/bin/python2.7 来在自动模式中提供 /usr/bin/python (python)

~$ sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 2
update-alternatives: 使用 /usr/bin/python3 来在自动模式中提供 /usr/bin/python (python)
```

**设置 python 默认版本：**

```
~$ sudo update-alternatives --config python

有 2 个候选项可用于替换 python (提供 /usr/bin/python)。

  选择       路径              优先级  状态
------------------------------------------------------------
  0            /usr/bin/python3     2         自动模式
  1            /usr/bin/python2.7   1         手动模式
* 2            /usr/bin/python3     2         手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：
```

**查看当前 python 版本：**

```
herald@homeserver:~$ python --version
Python 3.5.3
```

参考：[https://linuxconfig.org/how-to-change-default-python-version-on-debian-9-stretch-linux](https://linuxconfig.org/how-to-change-default-python-version-on-debian-9-stretch-linux)
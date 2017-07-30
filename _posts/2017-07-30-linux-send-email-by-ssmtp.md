---
title: Linux 使用 sSMTP 发送邮件
layout: post
date: '2017-07-30 20:53:54'
---

`sSMTP` is a simple MTA to deliver mail from a computer to a mail hub (SMTP server). sSMTP is simple and lightweight, there are no daemons or anything hogging up CPU; Just sSMTP. Unlike Exim4, sSMTP does not receive mail, expand aliases, or manage a queue. 

Debian 系统安装 `mailutils` 软件包，获得 `mail` 命令。

#### 安装 sSMTP 和 mailutils

```
~$ sudo apt install ssmtp mailutils
```

#### sSMTP 配置样本

```
~$ sudo nano /etc/ssmtp/ssmtp.conf

root=postmaster
mailhub=smtpdm.aliyun.com:465
AuthUser=notice@getnas.com
AuthPass=PassWord
UseTLS=YES
hostname=HomeServer
FromLineOverride=NO
```

`FromLineOverride` 为 `YES` 则发件人显示为系统主机名为后缀的伪地址，同时显示由 XXX 代发。设置为 `NO` 则显示真实的邮件地址。

#### 修改 From Address

一般外部 smtp 服务器要求发件人为真实地址，否则会报错。

```
~$ sudo nano revaliases

root:notice@getnas.com
herald:notice@getnas.com
```

#### 发送测试邮件

```
~$ echo "hello world!" | mail -s "Test E-mail" testing@163.com
```

#### 排错

发件失败时会提示 `mail: cannot send message: Process exited with a non-zero status`，查看 `mail.log` 文件查找错误原因：

```
~$ sudo nano /var/log/mail.log
```


参考一：[ssmtp](https://wiki.debian.org/sSMTP)

参考二：[msmtp](https://wiki.debian.org/msmtp)

`mSMTP` is an SMTP client that can be used to send mails from Mutt and probably other MUAs (mail user agents). It forwards mails to an SMTP server (for example at a free mail provider), which takes care of the final delivery. Using profiles, it can be easily configured to use different SMTP servers with different configurations, which makes it ideal for mobile clients.
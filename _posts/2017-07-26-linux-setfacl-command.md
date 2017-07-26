---
title: setfacl 命令
layout: post
date: '2017-07-26 16:56:47'
---

ACL = Access Control Lists 访问控制列表

`setfacl` 是在命令行设置 ACL 的工具

```
herald@homeserver:~$ setfacl --help

setfacl 2.2.52 -- set file access control lists
Usage: setfacl [-bkndRLP] { -m|-M|-x|-X ... } file ...
  -m, --modify=acl        modify the current ACL(s) of file(s)
  -M, --modify-file=file  read ACL entries to modify from file
  -x, --remove=acl        remove entries from the ACL(s) of file(s)
  -X, --remove-file=file  read ACL entries to remove from file
  -b, --remove-all        remove all extended ACL entries
  -k, --remove-default    remove the default ACL
      --set=acl           set the ACL of file(s), replacing the current ACL
      --set-file=file     read ACL entries to set from file
      --mask              do recalculate the effective rights mask
  -n, --no-mask           don't recalculate the effective rights mask
  -d, --default           operations apply to the default ACL
  -R, --recursive         recurse into subdirectories
  -L, --logical           logical walk, follow symbolic links
  -P, --physical          physical walk, do not follow symbolic links
      --restore=file      restore ACLs (inverse of `getfacl -R')
      --test              test mode (ACLs are not modified)
  -v, --version           print version and exit
  -h, --help              this help text
```

例如，查看一个目录或文件的 ACL 设置

```
herald@homeserver:~$ getfacl ./mydata/

# file: mydata
# owner: herald
# group: herald
user::rwx
group::r-x
other::r-x
```

如果要实现 `herald` 用户对某个目录下所有新建的目录和文件都有 `rwx` 权限：

```
herald@homeserver:~$ sudo setfacl -d -m user:herald:rwx sync/
```

参考：http://man.linuxde.net/setfacl
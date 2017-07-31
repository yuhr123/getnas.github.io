---
title: setfacl 和 getfacl 命令
layout: post
date: '2017-07-26 16:56:47'
---

ACL = Access Control Lists 访问控制列表，用于更细粒度的文件权限控制。

`setfacl` 用于设置 ACL，`getfacl` 用于查看 ACL 设置。

```
~$ setfacl --help

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

**例如，查看一个 `sync` 目录的 ACL 设置：**

```
~/mydata$ getfacl ./sync

# file: mydata
# owner: herald
# group: herald
user::rwx
group::r-x
other::r-x
```

**让 `easy` 用户对 `sync` 目录及子目录/文件拥有读写执行权限：**

```
~/mydata$ setfacl -m user:easy:rwx sync/

~/mydata$ getfacl sync/

# file: sync/
# owner: herald
# group: herald
user::rwx
user:easy:rwx
group::r-x
mask::rwx
other::r-x
```

如果希望将 `sync` 中原有的目录/文件也做类似设置，则使用 `setfacl -R -m` 递归处理。

此外，如果想让 `easy` 用户在 `sync` 目录下创建的文件/目录的组继承 `sync` 目录的设置即 `herald` 组：

```
~/mydata$ chmod g+s sync/
```
---
title: Linux 文件去重工具 FDUPES
layout: post
date: '2017-08-08 08:16:44'
---

### FDUPES

`fdupes` 用于检查或删除指定目录中的重复文件。

项目主页：[https://github.com/adrianlopezroche/fdupes](https://github.com/adrianlopezroche/fdupes)

#### 安装

```
~$ sudo apt install fdupes
```

####  基本用法

以当前用户家目录下 `pic` 文件夹为例

检查重复文件：
```
~$ fdupes ~/pic
```

递归检查目录及子目录中的重复文件：
```
~$ fdupes -r ~/pic
```

检查并交互式删除重复文件：
```
~$ fdupes -d ~/pic
```

查看重复文件的大小：
```
~$ fdupes -s ~/pic
```

显示检查的统计结果：
```
~$ fdupes -m ~/pic
```
---
title: "linux tmp文件"
date: 2023-06-18
draft: false
tags : [                    # 文章所属标签
    "Linux",
]
categories : [              # 文章所属标签
    "技术",
]
---

tmp是temporary的缩写，这个目录是用来存放一些临时文件。`/tmp`是Linux下的临时文件夹。

该文件夹中的内容一般不会删除，以redhat为例，系统自动清理`/tmp`文件夹的默认时限是30天。30天不访问的`/tmp`下的文件会被系统自动删除。

`/tmp`-临时文件目录，能够被任何用户，任何程序访问，一般用来存放程序的临时文件，所以应该定期清理一下。FHS甚至建议在开机时，应该要将`/tmp`下的数据都删除，临时目录还有`/var/tmp`。

Linux有两个公认的临时目录：`/tmp`与`/var/tmp`，这两个目录被用户用于存储临时性的文件，亦经常被程序读写用户存储临时性数据。
两个目录没有本质上的区别，最根本的区别仅仅是系统对其中文件清理的默认时间配置不一致。
- `/tmp`：目录默认清理**10**天未用的文件，系统重启会清理目录;
- `/var/tmp`：目录默认清理**30**天未用的文件。

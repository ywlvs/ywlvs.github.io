---
layout:     post
title:      "Linux 中的压缩工具 tar"
subtitle:   ""
date:       2019-10-30 11:20:19
author:     "ywlvs"
header-img: "img/post-bg-docker-logo-whale.png"
catalog: true
tags:
    - tar
    - linux
---

tar 是 linux 发行版中常用的压缩工具，方便好用。

## 用法

tar [选项...] [FILE] ...


## 常用选项

+ `-v, --verbose` 详细地列出处理的文件

+ `-t, --list` 列出归档内容

+ `-f` 使用归档文件或 ARCHIVE 设备

+ `-c, --create` 创建一个新归档

+ `-x， --extract， --get` 从归档中解出文件

+ `-z, --gzip, --ungzip, --gunzip` 通过 gzip 过滤归档

```
tar -cf archive.tar foo.txt bar.avi     # 将 foo.txt 和 bar.avi 打包压缩为 archive.tar 文件

tar -tvf archive.tar    # 详细列举归档文档 archive.tar

tar -czf archive.tar.gz foo.txt bar.avi # 将 foo.txt 和 bar.avi 打包并使用 gzip 过滤归档。注意 -z 参数的使用和归档文件的扩展名。

tar -xzf archive.tar.gz 从归档文件中解出文件
```
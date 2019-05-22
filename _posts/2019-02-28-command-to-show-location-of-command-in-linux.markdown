---
layout:     post
title:      "Linux 中如何查看命令文件的位置"
subtitle:   "Show Command Location in Linux"
date:       2019-02-28 10:02:00
author:     "ywlvs"
header-img: "img/post-bg-sequences.png"
catalog: true
tags:
    - computer
    - linux
---

## type

type 用来显示给定命令的类型，并判断该命令是内部指令还是外部指令

### 语法

type [选项] 参数

### 可选选项

+ `-t` 输出命令的类型

+ `-p` 如果给定的命令是外部指令，则显示该命令的绝对路径

+ `-f` 不搜索 shell 函数

+ `-a` display all locations containing an executable named NAME; includes aliases, builtins, and functions, if and only if the `-p` option is not also used


+ `-P` force a PATH search for each NAME, even if it is an alias, builtin, or function, and returns the name of the disk file that would be executed
```

### 使用 -t 选项输出的类型

+ alias：别名。
+ keyword：关键字，Shell保留字。
+ function：函数，Shell函数。
+ builtin：内建命令，Shell内建命令。
+ file：文件，磁盘文件，外部命令。
+ unfound：没有找到。


```
type php7.2

type -p php7.2

type -p php7.2 mysql vim
```

## which

```
which php7.2

which php7.2 mysql vim
```

## whereis

```
whereis php7.2
```
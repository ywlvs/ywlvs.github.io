---
layout:     post
title:      "查看 Linux 内核信息的常用命令"
subtitle:   "Commands to Show Kernel Info of Linux"
date:       2018-01-02 22:00:00
author:     "ywlvs"
header-img: "img/post-bg-assistant-for-linux-and-mac.jpg"
catalog: true
tags:
    - Linux
---

> #### 在使用 `Linux` 的过程中，经常需要查看当前版本的内核信息，今天记录一些常用的相关命令。

#### Linux 内核版本命令

> 以下两条命令适用于所有的 `Linux` 发行版本。

**`cat /proc/version`**

```
$ cat /proc/version
Linux version 4.4.0-97-generic (buildd@lcy01-33) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #120-Ubuntu SMP Tue Sep 19 17:28:18 UTC 2017
```

**`uname -a`**

```
$ uname -a
Linux PC 4.4.0-97-generic #120-Ubuntu SMP Tue Sep 19 17:28:18 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
```

#### Linux 系统版本的命令

**`lsb_release -a`**

> 该命令适用于所有的 `Linux` 发行版，包括 `Redhat`, `OpenSuSe` 等发行版本。有的系统中默认并没有安装 `lsb_release`，需要安装。

```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.3 LTS
Release:    16.04
Codename:   xenial
```

**`cat /etc/issue`**

> 此命令适合所有的 `Linux` 发行版。

```
$ cat /etc/issue
Ubuntu 16.04.3 LTS \n \l
```

**`cat /etc/redhat-release`**

> 此命令只适合查看 `Red Hat` 系的发行版本，如：`CentOS`。

```
$ cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core)
```

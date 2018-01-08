---
layout:     post
title:      "Laravel 中日志文件的权限问题"
subtitle:   "Something about Access to Log Files in Laravel"
date:       2018-01-08 21:10:00
author:     "ywlvs"
header-img: "img/post-bg-the-world-of-web-characters.jpg"
catalog: true
tags:
    - Linux
    - Laravel
    - Crontab
---

>在平常的开发中，日志可以记录应用出现的一些错误，也可以记录一些操作结果。`Laravel` 使用功能强大的 `Monolog` 库进行日志处理，使用起来非常方便。在使用过程中，使用 `Log` 的 `Facade` 可以轻松实现各种记录的需求。然而，有的时候却会遇到类似 `(log file) could not be opened: failed to open stream: Permission denied` 的错误。

### 问题分析

这是权限的问题，执行当前线程的用户（user）没有相关日志文件的操作权限，这里分两种情况。

+ 日志文件已经存在，无法在已有日志文件中追加内容。因为 `Linux` 是一个多用户的操作系统，出于安全的考虑，不同的用户有不同的权限，比如属于用户 A 的文件，用户 B 一般是没有操作权限的，尤其是写入的权限。

+ 不存在日志文件，这是无法写新文件。

### 解决方案

>以前遇到这种问题，基本就是使用 `chmod` 命令更改相应文件或者目录的操作权限，甚至是将已存在的文件删除，也确实是立杆见影，命令执行完错误也不见了，然而指不定什么时候，熟悉的错误就有回来了，这就说明之前的操作治标不治本呀，没有从根本上解决问题。

#### 无法创建日志文件的解决方案

这种问题其实好办，无法创建目录的问题一般出现在项目的创建初期，可能是 `storage` 目录的权限没有放开，只要执行以下 `chmod` 命令赋予修改一下权限就可以了。一般不建议直接将文件的权限属性改成 `777`，据说这样会有一些安全上的隐患。这个命令执行完，新创建日志文件的时候，基本不会再出现权限相关的错误了。

#### 追加日志内容时无权限的解决方案

追加日志内容时的权限问题，基本是发生在应用有使用 Artisan 命令行执行任务调度的时候。一般我们用 `supervisor` 管理进程，进而实现定时任务的执行，其本质依旧是使用 `crontab` 来实现的，而执行 `crontab` 线程的默认用户是 `root`，而普通 `web` 访问时执行 `php-fpm` 线程的通常不是 `root` 用户，可能是 `nobody` 用户。如果一个日志文件是由 `root` 用户创建的，`nobody` 用户当然没有该文件的操作权限，自然运行的时候会出现错误。

当问题的原因分析清楚之后，解决方案就不那么难了，曾经傻乎乎的使用 `chmod` 命令去修改日志文件的访问权限，简直可笑呀，现在不能再那么做了。

在 Google 上一番搜索之后，终于找到了比较优雅的解决方案。

在使用 `crontab` 设置定时任务时，其实是可以指定用户的。默认情况下，`crontab` 处理的是 `root` 用户的任务，我们可以通过追加 `-u` 选项来指定用户。

**`crontab -u nobody -l`**

>显示 `nobody` 用户的用户列表

```shell
$ crontab -u nobody -l

* * * * * cd /var/www/StrawberryApi && /usr/local/bin/php /var/www/StrawberryApi/artisan schedule:run >> /dev/null 2>&1
* * * * * cd /var/www/Inquiry_Patient && /usr/local/bin/php /var/www/Inquiry_Patient/artisan schedule:run >> /dev/null 2>&1
```

**`crontab -u nobody -e`**

>编辑修改 `nobody` 用户的任务列表

```shell
$ crontab -u nobody -e

* * * * * some-command-you-want-to-add-for-user-nobody
```

在获知 `crontab` 的这个功能之后，问题就可以十分优雅的解决了。将报错时所执行的任务从 `root` 用户的任务列表中删除，并加入到 `nobody` 用户的任务列表，然后就好咯。
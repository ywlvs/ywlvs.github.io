---
layout:     post
title:      "开启 Ubuntu 下 crontab 的执行日志"
subtitle:   "Activate Log of Crontab in Ububtu"
date:       2018-03-14 10:04:00
author:     "ywlvs"
header-img: "img/post-bg-php-magic-methods.jpg"
catalog: true
tags:
    - crontab
    - Linux
    - log
---


crontab 是执行定时任务的利器。然而，与 CentOS 不同的是，在 Ubuntu 系统中，crontab 默认是不开启日志的，没有日志文件，心里总是会感觉不踏实。在网上找到了开启 Ububtu 中记录日志的方法，记录下来。

---

### 参考文献

[Ubuntu cron日志开启与查看的实现步骤](http://www.jb51.net/article/125417.htm)

---

### 操作步骤

+ 修改系统日志配置文件，取消 crontab 日志配置的注释

```
vim /etc/rsyslog.d/50-default.conf
# cron.* /var/log/cron.log //将行首的 # 号去掉

cron.* /var/log/cron.log //去掉之后的样子
```

+ 重启 rsyslog

```
service rsyslog restart
```

+ 查看 crontab 日志

```
tail /var/log/cron.log
```

>只有 crontab 执行了定时任务，才会有日志文件。


---


默认cron会发送邮件把任务的运行结果比如错误信息发送到系统用户的邮箱中，而不会在log中出现具体的脚本运行输出的信息，所以我们可以指定一下每个任务的输出日志路径：

0 2 * * * db_backup task; >> /var/log/db_backup.log 2>&1

这样我们就可以去相应的日志文件查看脚本输出结果了。

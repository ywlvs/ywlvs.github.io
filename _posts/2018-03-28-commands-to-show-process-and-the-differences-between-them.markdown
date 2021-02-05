---
layout:     post
title:      "top, ps 和 htop 的应用与区别"
subtitle:   "Commands to Show Process and the Differences Between Them"
date:       2018-03-28 12:00:00
author:     "ywlvs"
header-img: "img/post-bg-dark-world.jpg"
catalog: true
tags:
    - Linux
    - top
    - ps
    - htop
---

top 与 ps 还有 htop 命令都可以用来查看系统的进程信息，当然，它们也是有区别的。

---

简单来说，使用 ps 命令，显示的是查询时刻的瞬间状态，显示查询结果，不会刷新。而使用 top 命令，查看的信息是动态的，也就是说，top 命令进入了一个程序的交互界面，我们可以查看系统运行的实时状态，并进行自己的操作，直到手动退出。而 htop 命令用来查看单个进程的线程。

### top 命令的使用

PID 进程编号
UID 有效用户编号，即展示出来的身份可能不是真实身份
USER 有效的用户名


* 进程� = Process Id               nDRT    = Dirty Pages Count
* USER    = Effective User Name    WCHAN   = Sleeping in Function
* PR      = 优先级                 标志  = Task Flags <sched.h>
* NI      = Nice Value             CGROUPS = Control Groups
* RES     = Resident Size (KiB)    SUPGIDS = Supp Groups IDs
* VIRT    = Virtual Image (KiB)    SUPGRPS = Supp Groups Names
* SHR     = Shared Memory (KiB)    TGID    = Thread Group Id
* 日     = Process Status          OOMa    = OOMEM Adjustment
* %CPU    = CPU Usage              OOMs    = OOMEM Score current
* %MEM    = Memory Usage (RES)     ENVIRON = Environment vars
* TIME+   = CPU Time, hundredths   vMj     = Major Faults delta
* COMMAND = Command Name/Line      vMn     = Minor Faults delta
  PPID    = Parent Process pid     USED    = Res+Swap Size (KiB)
  UID     = Effective User Id      nsIPC   = IPC namespace Inode
  RUID    = Real User Id           nsMNT   = MNT namespace Inode
  RUSER   = Real User Name         nsNET   = NET namespace Inode
  SUID    = Saved User Id          nsPID   = PID namespace Inode
  SUSER   = Saved User Name        nsUSER  = USER namespace Inode
  GID     = Group Id               nsUTS   = UTS namespace Inode
  GROUP   = Group Name             LXC     = LXC container name
  PGRP    = Process Group Id       RSan    = RES Anonymous (KiB)
  TTY     = Controlling Tty        RSfd    = RES File-based (KiB)
  TPGID   = Tty Process Grp Id     RSlk    = RES Locked (KiB)
  SID     = Session Id             RSsh    = RES Shared (KiB)
  nTH     = Number of Threads      CGNAME  = Control Group name
  P       = Last Used Cpu (SMP)
  时间  = CPU Time
  SWAP    = Swapped Size (KiB)
  CODE    = Code Size (KiB)
  DATA    = Data+Stack (KiB)
  nMaj    = Major Page Faults
  nMin    = Minor Page Faults

### ps 命令的使用

### htop 命令的使用

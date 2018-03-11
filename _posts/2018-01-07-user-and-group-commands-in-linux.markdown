---
layout:     post
title:      "Linux 中用户组及用户管理的命令"
subtitle:   "User Management Commands in Linux"
date:       2018-01-07 16:30:00
author:     "ywlvs"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
    - Linux
---

### Linux 中单用户多任务、多用户多任务的概念

#### 单用户多任务

比如我们登录了 Ubuntu 操作系统，我们要使用 Sublime 写代码，可能还有些无聊，就顺便使用 Rhythm 播放音乐，同时还要打开 Chrome 查看文档。那么，对于当前的登录的一个用户，系统要执行多个任务以满足用户的工作需求，这就是单用户多任务。

#### 多用户多任务

有的时候，登录一个系统的不只有一个用户。比如说我们有一个阿里云的 Ubuntu 服务器，小明通过 ftp 连接到服务器处理文件，小强通过 ssh 的方式也连接到了服务器的终端，而他们都是用自己的账号，在同一台机器上进行着不同的操作。这种方式就是多用户多任务，多用户多任务也可能是通过远程登录来实现，比如对服务器的远程控制。

#### 用户的角色区分

用户在系统中是分角色的，在 Linux 系统中，角色不同的用户，权限和所完成的任务也是不同的。用户的角色是通过 UID 来 GID 识别的。Linux 系统中，用户主要有三种：

1.**root 用户**：系统唯一，是真实的，可以登录系统，可以操作系统任何文件和命令，拥有最高权限。

2.**虚拟用户**：这类用户也被称之为伪用户或假用户，与真实用户区分开来，这类用户不具有登录系统的能力，但却是系统运行不可缺少的用户，比如 bin、daemon、adm、ftp、mail 等。这类用户都是系统自身拥有的，而非后来添加的，当然我们也可以添加虚拟用户。

3.**普通真实用户**：这类用户能登录系统，但只能操作自己家目录（home）的内容，权限有限。这类用户都是系统管理员自行添加的。

### 用户（user）和用户组（group）的概念

**用户（user）**

Linux 是多用户的操作系统，我们可以在系统中根据需要添加多个用户。比如小明和小强要使用同一台电脑，但是彼此都一些隐私的内容不想被他人看到，那么他们可以通过管理员账号为每个人分配一个独立的用户，小明和小强分别使用自己的账号登录系统，均只能看到属于自己的文件，这样就一定程度上保证了私人文件的安全。

当然，用户（user）的概念不仅如此，在 Linux 中，还有一些用户是用来完成特定任务的，就是上文提及的虚拟用户，如 ftp 和 nobody 等。我们访问网页时，就是 nobody 用户，匿名访问 ftp 时，会使用到 ftp 或者 nobody。

**用户组（group）**

用户组（group）是具有相同特征的一些用户（user）的集合。

**用户（user）和用户组（group）的关系**

用户和用户组的对应关系可以是：一对一，多对一，一对多或多对多。

1.一对一：

某个用户可以是某个组的唯一成员；

2.多对一：

多个用户可以是某个唯一用户组的成员，不属于其他用户组。比如 xiaoming 和 xiaoqiang 两个用户只属于 mate 用户组；

3.一对多：

某个用户可以是多个用户组的成员，比如 xiaoming 用户可以是用户组 mate 的成员，也可以是用户组 fridends 的成员，也可以是用户组 students 的成员。

4.多对多：

多对多的关系就是上面三个关系的扩展，不同组之间的成员可以有交集。

### 用户（user）和用户组（group）相关的配置文件

1.与用户（user）相关的配置文件

/etc/passwd：用户（user）的配置文件

/etc/shadow：用户（user）影子口令文件

2.与用户组（group）相关的配置文件

/etc/group：用户组（group）配置文件

/etc/gshadow：用户组（group）的影子文件


### 相关命令

创建用户的命令有：adduser 和 useradd。

+ adduser：会自动为创建的用户指定主目录、系统 shell 版本，会在创建时输入用户密码。

+ useradd：需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定 shell 版本。

#### 使用 adduser

```
root@Simona:~# adduser george
Adding user `george' ...
Adding new group `george' (1000) ...
Adding new user `george' (1000) with group `george' ...
Creating home directory `/home/george' ...
Copying files from `/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Changing the user information for george
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n] y

```

默认情况下，adduser 在创建用户时会主动调用 /etc/adduser.conf；

在创建用户主目录时，默认在 /home 下，而且创建为 /home/用户名。如果主目录已经存在，则不再创建，但是此主目录虽然作为新用户的主目录，而且默认登录时会进入这个目录下，但是这个目录并不是属于新用户，当使用 userdel 删除该用户时，并不会删除这个主目录，因为该目录在创建该用户之前就已经存在。

查看 shell 版本

```
george@Simona:~$ echo $SHELL
/bin/bash
```

常用参数选项为：

+ --home：指定创建主目录的路径，默认是在 /home 目录下创建同名的目录，这里可以指定；如果主目录同名目录存在，则不再创建，仅在登录时进入主目录。

+ --quiet：只打印警告和错误信息，忽略其他信息。

+ --debug：定位错误信息。

+ --conf：在创建用户时使用指定的 configuration 文件。

+ --force-badname：默认在创建用户时会进行 /etc/adduser.conf 中的正则表达式检查用户名是否合法，如果想使用弱检查，则使用这个选项，如果不想检查，可以将 /etc/adduser.conf 中相关选项屏蔽。如

```
# check user and group names also against this regular expression.
#NAME_REGEX="^[a-z][-a-z0-9_]*\$"
```

#### 使用 useradd

不使用任何参数选项创建用户

```
root@Simona:~# useradd paul
```

正确执行完上述语句后，不会有任何输出，需要手动为该用户设置密码

```
root@Simona:/home# passwd paul
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

```

使用 ssh 远程连接时，会发现没有为用户创建默认的主目录。

```
[c:\~]$ ssh paul@120.78.201.5

Connecting to 120.78.201.5:22...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


Welcome to Alibaba Cloud Elastic Compute Service !


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Could not chdir to home directory /home/paul: No such file or directory
$
```

用户登录后的所在目录是根目录 /，虽然 $HOME 环境变量为 /home/paul

```
$ echo $HOME
/home/paul
```

查看 shell 版本，发现是 /bin/sh，说明没有指定 shell 版本。

```
$ echo $SHELL
/bin/sh
```

常用参数选项包括：

+ -d：指定用户的主目录

+ -m：如果存在不再创建，但是此目录并不属于新创建用户；如果主目录不存在，则强制创建；-m 和 -d 一同使用。

+ -s：指定用户登录时的 shell 版本。

+ -M：不创建主目录。


### 删除用户的命令： userdel

+ 只删除用户

```
root@Simona:~# userdel paul
```

+ 连同用户主目录一同删除。如果创建用户之前，主目录已经存在，即主目录不属于当前要删除的用户，则无法删除主目录。

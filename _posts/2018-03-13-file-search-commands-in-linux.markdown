---
layout:     post
title:      "Linux 系统文件搜索命令之 find"
subtitle:   "command for file searching in Linux : find"
date:       2018-11-13 19:00:00
author:     "ywlvs"
header-img: "img/post-bg-lars-sowig-final-silver.jpg"
catalog: true
tags:
    - Linux
    - find
    - search
---


### find 查找文件

```
find [-H] [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]
```

> 在不指定查找范围时，find 命令默认在当前目录进行搜索。使用 `-name` 选项根据文件名称进行查找时，文件名需要加引号。

+ `-print`：find 命令将匹配的文件输出到标准输出，一般情况下就是我们的显示屏
+ `-exec`：find 命令对匹配的文件执行该参数所给出的 shell 命令。相应命令的形式为 'command' { } \;，注意{ }和\；之间的空格。
+ `-ok`：和 `-exec` 的作用相同，只不过以一种更为安全的模式来执行该参数所给出的 shell 命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。
+ `-name`：查找名为 filename 的文件

+ `-perm`：按执行权限来查找

+ `-user`：username，按文件属主来查找

+ `-group`：groupname，按组来查找

+ `-nogroup`：查无有效属组的文件，即文件的属组在/etc/groups中不存在

+ `-nouser`：查无有效属主的文件，即文件的属主在/etc/passwd中不存

+ `-newer`：比较文件的最后更新时间，查找比指定文件更新的文件

+ `-type`：限定查找文件的类型，b 表示块设备，d 表示目录，c 表示字符设备，p 表示管道，l 表示符号链接，f 表示普通文件

+ `-size`：使用文件长度限定查找范围，文件长度可以用块或者字节来衡量，使用块做单位时，直接用数字表示即可，使用字节作为单位时，数字后跟一个字符 c 即可，我们使用 ls -l 命令列出文件的时候，显示文件大小的默认单位就是字节。

+ `-maxdepth`：限定查找目录的层级，默认是所有的层级

+ `-mtime`：-n +n，指定文件内容修改的时间，-n指n天以内，+n指n天以前

+ `-atime`：-n +n，指定文件被读取或者执行的时间

+ `-ctime`：-n +n，指定文件被修改的时间，包括文件内容被修改和文件的元数据被修改

+ `-depth`：使查找在进入子目录前先行查找完本目录

+ `-fstype`：指定查找文件所属的文件系统类型，文件系统类型可以在 /etc/fstab 中查看

+ `-follow`：已经废弃，使用 `-L` 来代替。指定该选项时，如果遇到符号链接文件，则跟踪该链接所指向的文件

+ `-prune`：指定忽略的目录，不在该目录中查找文件
+ `-mount`：查找文件时，不跨越文件系统 mount 点
+ `-cpio`：对匹配的文件使用cpio命令，将他们备份到磁带设备中

+ `-empty`：查找空的文件

+ `-regex`：使用正则表达式进行搜索

+ `-iregex`：利用正则匹配，与 `-regex` 的区别是，这个选线对大小写不敏感

### 操作符

> 与编程语言相似，存在逻辑非、逻辑或以及逻辑与的操作符。例如逻辑或，当操作符左侧的表达式为真时，不会验证右侧表达时的真假。逻辑与的操作也和编程中逻辑操作类似。

+ `!`：逻辑非，作用于后面的条件表达式
+ `-not`：和 `!` 作用一致，但是不兼容 POSIX
+ `-a`：逻辑并与，操作符左右的表达式都要满足。该操作符时默认操作符，一般可不写
+ `-and`：和 `-a` 作用一致，但是不兼容 POSIX
+ `-o`：逻辑或
+ `-or`：作用同 `-o`，不兼容 POSIX。


### Linux 系统中的 atime，ctime 和 mtime

+ atime(access time)

    access，一般有「进入」的意思，atime 指的就是文件被执行、读取的时间。对于哪些操作能够改变 atime 这个参数，目前特别清楚。使用 cat、more、tail、head 等命令查看文件内容时，并不会更新该属性，使用 vim 仅仅查看文件内容，也不会更新该属性。对文件的修改，必然会更新该属性。

+ ctime(change time)

    这个改变时间，不仅限于文件内容的改变，文件的元数据（所有者和执行权限等）的变更，也会造成该属性的更新。

+ mtime(modified time)

    这个修改，一般指的是文件内容的修改，对文件元数据的修改（比如修改文件的权限）不会更新该属性。


###

+ 查找大小**等于** 100 字节的文件

    ```
    find -size 100c
    ```

+ 查找大小**大于** 100K 的文件

    ```
    find -size +100K
    ```

+ 查找大小**小于** 100 字节的文件

    ```
    find -size +100c
    ```

+ 查找大小**等于 100 块**的文件

    ```
    find -size 100
    ```

+ 限定查找层级为 1，即只查找当前目录

    ```
    find -maxdepth 1 -name '*.py'
    ```

+ 指定文件的修改时间，一天之内有修改过

    ```
    find -maxdepth 1 -ctime -1
    ```

+ 查找四十分钟内修改过的文件

    ```
    find -maxdepth 1 -cmin -40
    ```

+ 查找当前目录下 root 用户所拥有的文件

    ```
    find . -type f -user root -print
    ```

+ 查找具有可执行权限的文件

    ```
    find . -type f -perm 644 -print
    ```

+ 查找当前目录下的 txt 和 pdf 文件

    ```
    find . \( -name "*.txt" -o -name "*.pdf" \) -print
    ```

+ 正则方式查找 txt 和 pdf 文件

    ```
    find . -regex ".*\(\.txt|\.pdf\)$"
    ```

+ 查找非 txt 文件

    ```
    find . ! -name "*.txt" -print
    ```
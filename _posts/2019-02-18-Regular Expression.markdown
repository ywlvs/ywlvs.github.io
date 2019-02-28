---
layout:     post
title:      "学习正则表达式"
subtitle:   "Regular Expression"
date:       2019-02-25 10:02:00
author:     "ywlvs"
header-img: "img/post-bg-sequences.png"
catalog: true
tags:
    - computer
    - regex
---

<style>
    .red {
        color: red;
    }
</style>

### 基本匹配

+ `\<` 表示锚定词首

+ `\>` 表示锚定词尾

+ `\b` 既能锚定词首，也能锚定词尾

+ `\B` 与 `\b` 相反，`\b` 使用来匹配「单词边界」的，但是 `\B` 刚好相反
    > 比如有 morning 和 goodmorning 两个字符串，***\bmorning*** 可以匹配到 morning，无法匹配到 goodmorning, 而 **_\Bmorning_** 匹配到的是 goodmorning，而不会匹配到 morning。

+ ^ 表示锚定行首

+ $ 表示锚定行尾


### 次数匹配

> 根据指定字符的连续出现次数进行匹配

+ `{n}` 表示指定的字符连续出现 n 次

+ `{m,n}` 字符连续出现的次数介于 m 和 n 之间

+ `{,n}` 字符连续出现的次数不高于 n，下限不做限制

+ `{m,}` 字符连续出现的次数不低于 m，上线不做限制

+ `*` 匹配任意长度的字符，包括长度为 0

> 如 `abc*` 能匹配 ab, abc, abcc, abccc 等。

+ `.` 匹配任意单个字符，换行符除外

> 如 '.' 能匹配 a, b, c, d 等

+ `.*` 则用来匹配任意长度的任意字符

+ `\?` 匹配规则为：字符出现 0 次或者 1 次

+ `\+` 匹配规则为：字符至少出现 1 次

+ `[]` 匹配范围内的任意单个字符

+ `[^]` 匹配范围外的单个字符


### 字符类型的匹配

+ `[[:alpha:]]` 匹配任意单个字母，不区分大小写，与 `[a-zA-Z]` 等效。

+ `[[:digit:]]` 匹配任意单个 0-9 的单个数字，与 `[0-9]` 等效。

+ `[[:lower:]]` 匹配任意单个小写字母，与 `[a-z]` 等效。

+ `[[:upper:]]` 匹配任意单个大写字母，与 `[A-Z]` 等效。

+ `[[:alnum:]]` 匹配任意单个字母或者数字，与 `[a-zA-Z0-9]` 等效。

+  `[[:space:]]` 匹配任意单个空白字符，包括「空格」「Tab制表符」等。

+ `[[:punct:]]` 匹配任意单个标点符号。

### 分组与后向引用

#### 啥是分组

举个例子。

有个文档 list_1，我们查看一下内容。

```
root@Simona:~# cat -n list_1

    1  goodgoodverygood

    2  goodddgoodgood

    3  123412341234
```

比如想找到连续出现两次的 good 字符串，该如何操作呢？使用 `\{n\}` 可以进行次数匹配，但是它表示该匹配符前面的单个字符重复出现 n 次，因此使用 `grep 'good\{2\}' list_1` 无法实现我们的目标。

```
root@Simona:~# grep -n 'good\{2\}' list_1

2:goodddgoodgood
```

该命令匹配的是第 2 行的 <span class="red">goodd</span>dgoodgood

上面对内容的匹配，都是针对单个字符的。如果要匹配某个单词重复若干次，这个时候使用分组就对了。


```
root@Simona:~# grep -n '\(good\)\{2\}' list_1

1:goodgoodverygood

2:goodddgoodgood
```

使用上面的匹配模式，匹配结果是 <span class="red">goodgood</span>verygood 以及 gooddd<span class="red">goodgood</span>。

分组，就是将若干连续的字符视为一个整体作为匹配的对象。上面例子中，就是将 good 四个字符分成一组进行匹配的。

`\(\)` 就是分组匹配符，`\(good\)` 就是将 good 作为一组去匹配。

#### 啥是向后引用

所谓向后引用，就是利用前面分组所匹配的对象，对后面的内容进行匹配。

举个例子。查看一下文件 list_2 的内容

```
root@Simona:~# cat -n list_2

    1  god & gun

    2  god & god
```

使用下面的匹配规则

```
root@Simona:~# grep -n '.\{3\} & .\{3\}' list_2

1:god & gun

2:god & god
```

可以看到，该模式会将两行都匹配出来。如果我们只想匹配第 2 行呢，就是说，对于该文本的每一行，要求 & 符号前后的字符串一致。这个时候可以使用的向后匹配。


```
root@Simona:~# grep -n '\(.\{3\}\) & \1' list_2

2:god & god
```

### 在 shell 中通过 grep 工具使用正则

#### 环境配置

    + Ubuntu 16.04.2

    + /bin/bash

#### 使用示例

+ 匹配一个 uuid 字符串

```
echo 9ea18301-3b7e-8e72-2566-7856cb543762 | grep '^[[:alnum:]]\{8\}-[[:alnum:]]\{4\}-[[:alnum:]]\{4\}-[[:alnum:]]\{4\}-[[:alnum:]]\{12\}$'
```

### 字符匹配规则的简写

+ `\d` 表示任意单个0到9的数字

+ `\D` 表示任意单个非数字字符

+ `\t` 表示匹配单个横向制表符（相当于一个tab键）

+ `\s` 表示匹配单个空白字符，包括「空格」，「tab制表符」等

+ `\S` 表示匹配单个非空白字符


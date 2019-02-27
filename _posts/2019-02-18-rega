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
    .command {
        color: red;
    }
</style>


### 基本匹配

+ `\<` 表示锚定词首
+ `\>` 表示锚定词尾
+ `\b` 既能锚定词首，也能锚定词尾
+ `\B` 与 `\b` 相反，`\b` 使用来匹配「单词边界」的，但是 `\B` 刚好相反
    > 比如有 morning 和 goodmorning 两个字符串，***\bmorning*** 可以匹配到 morning，无法匹配到 goodmorning, 而 **_\Bmorning_** 匹配到的是 goodmorning，而不会匹配到 morning。

+ <span class="command">hahaa</span>

+ ^ 表示锚定行首
+ $ 表示锚定行尾


### 次数匹配

> 根据指定字符的连续出现次数进行匹配

+ `{n}` 表示指定的字符连续出现 n 次
+ `{m,n}` 字符连续出现的次数介于 m 和 n 之间
+ `{,n}` 字符连续出现的次数不高于 n，下限不做限制
+ `{m,}` 字符连续出现的次数不低于 m，上线不做限制
+ `*` 匹配任意长度的字符，包括长度为 0

> 如 'abc*' 能匹配 ab, abc, abcc, abccc 等。

+ `.` 匹配任意单个字符，换行符除外

> 如 '.' 能匹配 a, b, c, d 等

+ `.*` 则用来匹配任意长度的任意字符

+ `\?` 匹配规则为：字符出现 0 次或者 1 次

+ `\+` 匹配规则为：字符至少出现 1 次

+ `[]` 匹配范围内的任意单个字符

+ `[^]` 匹配范围外的单个字符

+ `[[:alpha:]]`

+ `[[:digit:]]`

+ `[[:lower:]]`

+ `[[:upper:]]`

+ `[[:alnum:]]`


### 在 shell 中通过 grep 工具使用正则

#### 环境配置
    + Ubuntu 16.04.2
    + /bin/bash

#### 使用示例

+ 匹配一个 uuid 字符串

```
echo 9ea18301-3b7e-8e72-2566-7856cb543762 | grep '^[[:alnum:]]\{8\}-[[:alnum:]]\{4\}-[[:alnum:]]\{4\}-[[:alnum:]]\{4\}-[[:alnum:]]\{12\}$'
```


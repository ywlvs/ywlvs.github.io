
---
layout:     post
title:      "MySql 中常见的数据类型"
subtitle:   "Data Types in MySql"
date:       2017-12-24 22:00:00
author:     "ywlvs"
header-img: "img/post-bg-mysql-logo.png"
catalog: true
tags:
    - MySql
---

> #### 数据类型是 MySql 的基础，了解各种类型数据的区别以及作用，对数据库的设计以及操作都有很大的帮助。

### **`int`**

>`int` 是 MySql 中常见的数据类型，具体又可以分成 `bigint`,`int`,`smallint` 和 `tinyint` 四种，这四种类型主要是存储所占空间不同，可表示的范围也不一样。

+ `bigint` 存储需要 8 字节空间，可表示的范围是从 -2^63 ~ 2^63-1

+ `int` 存储需要 4 字节空间，可表示的范围是从 -2^31 ~ 2^32

+ `smallint` 存储需要 2 字节空间，可表示范围是 -2^15 ~ 2^16

+ `tinyint` 存储需要 1 字节空间，可表示范围是 -2^7 ~ 2^8

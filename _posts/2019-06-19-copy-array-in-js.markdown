---
layout:     post
title:      "ES6 中的数组拷贝"
subtitle:   "Copy Array in ES6"
date:       2019-06-19 10:57:00
author:     "ywlvs"
header-img: "img/post-bg-borealis-blue-cabin.jpg"
catalog: true
tags:
    - ES6
    - Array
---

今天偶然看到了 ES6 中的编程风格，里面提到了使用扩展运算符（...）拷贝数组，感到挺好奇的，因为平时很少接触 JS，一直以为和 PHP 类似，数组拷贝靠赋值就行了，原来并不是这样。


```?JavaScript
numbers = [1, 2, 3, 4, 5]
numbersCopy = numbers
// numbers[1] 和 numbersCopy[2] 的值均为 2

numbersCopy[1] = 10
// 这个时候 numbers[1] 和 numbersCopy[1] 的值同时改变了，均为 10
```

通过上面的例子，可以发现，使用赋值的形式来拷贝数组，指向两个变量的指针是一样的，即两个变量拥有相同的数据。

那么，如何正确拷贝数组呢？

```?JavaScript
numbers = [1, 2, 3, 4, 5]
numbersCopy = [...numbers]
```

这样一来，numbers 和 numbersCopy 两个变量不再分享数据。
---
layout:     post
title:      "PHP 中 strlen 和 mb_strlen 函数的区别"
subtitle:   "Differences between strlen and mb_strlen"
date:       2017-12-26 20:00:00
author:     "ywlvs"
header-img: "img/post-bg-learn-php.jpg"
catalog: true
tags:
    - MySql
---

> #### `PHP` 中，`strlen` 和 `mb_strlen` 函数都可以用来计算字符串的长度，但是两个函数是有区别的。

### **`strlen`**

>使用 `strlen` 计算字符串的长度，返回的是字符串所占的字节数。

对于英文字符，每个字符的长度是 1。对于中文字符，使用 `GB2312` 编码的字符串，`strlen` 得到的值是汉字个数的 2 倍；使用 `UTF-8` 编码的字符串，`strlen` 计算得到的值是汉字个数的 3 倍，因为每个使用 `UTF-8` 编码的中文字符占 3 个字节。

```php

<?php

$str = 'abc';
echo strlen($str);  // 3

$str = 'ab c';
echo strlen($str);  // 4，中间有一个空格

$str = '学习';
echo strlen($str);  // 6，UTF-8 编码，每个字符是 3 个长度

$str = '学习PHP';
echo strlen($str);  // 9, 3 * 2 + 3

?>

```

### **`mb_strlen`**

>`mb_strlen` 计算给定字符串的长度，返回字符的个数。另外，该函数不是 `PHP` 的核心函数。

```
mixed mb_strlen ( string $str [, string $encoding = mb_internal_encoding() ] )
```

由该函数的定义可以看出，除了要检查的字符串以外，还可以传递第二个参数用以指定字符编码。如果省略第二个函数，则会使用 `PHP` 的内部编码，内部编码可以通过 `mb_internal_encoding()` 函数获得。

```php

<?php

$str = '学习PHP';

echo mb_strlen($str);               // 5
echo mb_strlen($str, 'UTF-8');      // 5
echo mb_strlen($str, 'GBK');        // 6
echo mb_strlen($str, 'gb2312');     // 6

?>

```

>对于第二个参数，应该是用指定的编码去解析宽字符吧，我猜的。
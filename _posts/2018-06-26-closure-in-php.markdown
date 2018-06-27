---
layout:     post
title:      "PHP 中的闭包"
subtitle:   "Closure-in-PHP"
date:       2018-06-26 20:00:00
author:     "ywlvs"
header-img: "img/post-bg-borealis-blue-cabin.jpg"
catalog: true
tags:
    - PHP
    - Closure
---

匿名函数，也叫闭包函数，允许临时创建一个没有名称的函数，经常用作回掉函数的参数的值。

```php

<?php

$names = ['Tom', 'Jerry', 'Roger'];

$callback = function ($name) {
    echo 'Hello ', $name, "\n";
};

array_walk($names, $callback);

?>

```

闭包函数也可以作为变量的值来使用，PHP 会自动把此种表达式转换成内置类 Closure 的对象实例。把一个 Closure 对象赋值给一个变量和普通变量赋值的语法是一样的，赋值语句后面要加上分号。如

```php
<?php

$greet = function ($message) {
    echo 'Hello' . ' ' . $message; 
};

$greet('XiaoMing');     //Hello XiaoMing

$greet('World');        //Hello World

?>
```

闭包函数可以从父作用域继承变量，任何此类变量都应该使用 use 语言结构传递进去。从 PHP7.1 期，$this、superglobals 和参数重名的变量不能传入进去。

```php
<?php

$message = 'How do you do';

$greet = function () {
    echo $message, "\n";
};

$greet();   //提示 message 变量未定义

$greet = function () use ($message) {
    echo $message, "\n";
};

$greet();   //从父作用域继承了变量，正确打印

$message = 'Good Morning';

$greet();   //依旧打印 How do you do，继承的变量值是闭包定义时确定的，而不是调用时确定的。

/**
 * 闭包也可以使用 引用 的方式进行传参
 */
$message = 'How do you do';     //重置参数值

$greet = function () use (&$message) {
    echo $message, "\n";
};

$greet();   //通过引用的方式继承变量，正确打印

$message = 'Good Night';

$greet();   //打印 Good Night，因为是通过引用传参，原地址所指向的值已经发生改变。

/**
 * 普通变量的传递不使用 use
 */
$greet = function ($name) use ($message) {
    echo $message, ' ', $name, "\n";
};

$greet('Tom');

?>

```
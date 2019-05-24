---
layout:     post
title:      "PHP 中的后期静态绑定"
subtitle:   "Late Static Binding in PHP"
date:       2018-08-07 16:41:01
author:     "ywlvs"
header-img: "img/post-bg-endless-subroad.jpg"
catalog: true
tags:
    - static
    - PHP
    - bindings
---

面试的时候，经常会被问到 static 这个关键字的作用，大概知道有个“后期静态绑定”的作用，但是当时只知道这个名词，至于究竟是干嘛的，并不是真的很清楚。今天偶然看到了相关的知识，就记录一下。


>「后期静态绑定」指的是，static:: 不再被解析为定义当前方法所在的类，而是在实际运行时计算的。也可以称之为“静态绑定”，因为它可以用于（但不限于）静态方法的调用。

下面有个简单的例子

```php
class A
{
    public static function myName()
    {
        return 'A';
    }

    public static function printName()
    {
        echo self::myName();
    }
}

class B extends A
{
    public static function myName()
    {
        return 'B';
    }
}

B::printName();
```

调用 `B::printName();` 打印的结果是 'A'。接下来，让我们修改一下 class A 中的 printName 方法，将 self 修改为 static。

```php
class A
{
    public static function myName()
    {
        return 'A';
    }

    public static function printName()
    {
        echo static::myName();
    }
}

class B extends A
{
    public static function myName()
    {
        return 'B';
    }
}

B::printName();
```

将 self 修改成 static 之后，执行打印的结果是 ’B'。这是因为，使用 self:: 关键字，或者 __CLASS__ 对当前类的引用，都是取决于定义当前方法 所在的类。而使用 static 关键字，则解除了 self 的这个限制。


```php
class A
{
    public static function myName()
    {
        return 'A';
    }

    public static function printName()
    {
        echo static::myName();
    }
}

class B extends A
{
}

B::printName();
```

再次修改之前的代码，`B::printName()` 操作的输出结果为 ’A'，这是因为 `B` 类中没有 `myName` 方法也没有 `printName` 方法。
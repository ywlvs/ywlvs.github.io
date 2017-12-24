---
layout:     post
title:      "PHP 中的魔术方法和魔术常量"
subtitle:   "Magic Methods and Magic Constants in PHP"
date:       2017-12-24 22:00:00
author:     "ywlvs"
header-img: "img/post-bg-php-magic-methods.jpg"
catalog: true
tags:
    - PHP
---
### 学习 PHP ，有些基础知识是必须要补一补的，今天记录一下 php 中魔术方法和魔术常量的一些知识，原文来自[严肃的博客](http://yansu.org/2014/04/27/magic-methods-and-magic-constants-in-php.html)。

---

#### 魔术方法（Magic methods）

PHP 中把以两个下划线 __ 开头的方法称为魔术方法。

+ __construct()，类的构造函数
+ __destruct()，类的析构函数
+ __call()，在对象中调用一个不可访问方法时调用该函数
+ __callStatic()，用静态方式调用类中一个不可访问方法时调用
+ __get()，获取类中某属性时调用该函数
+ __set()，设置类中的属性时调用该函数
+ __isset()，当对不可访问属性调用 isset() 或 empty() 时调用
+ __unset()，当对不可访问属性调用 unset() 时调用
+ __sleep()，执行 serialize() 时，会先调用这个函数
+ __wakeup()，执行 unserialize() 时，会先调用这个函数
+ __toString()，类被当成字符串处理时的回应方法
+ __invoke()，调用函数的方式调用一个对象时的回应方法
+ __set_state()，调用 var_export() 导出类时，此静态方法会被调用
+ __clone()，当对象复制完成时调用

**`__construct()` 和 `__destruct()`**

> 构造函数和析构函数的调用分别发生在类生命周期的开始和结束，在对象创建时调用构造函数，在对象消亡时调用析构函数。比如我们需要打开一个文件，在对象创建时打开一个文件，对象消亡时关闭该文件。

```php
<?php

class FileRead
{
    protected $handle = null;

    function __construct()
    {
        $this->handle = fopen('file-you-want-to-open');
    }

    function __destruct()
    {
        fclode($this->handle);
    }
}
```
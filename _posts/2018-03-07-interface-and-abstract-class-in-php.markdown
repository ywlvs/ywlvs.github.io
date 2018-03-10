---
layout:     post
title:      "PHP 中的接口类和抽象类"
subtitle:   "interface-and-abstract-class-in-php"
date:       2018-03-07 11:00:00
author:     "ywlvs"
header-img: "img/post-bg-universe.jpg"
catalog: true
tags:
    - Interface
    - Abstract
    - Trait
---

### `interface` - 接口

接口是通过 interface 关键字来定义的。接口不是类，但是，使用接口可以限定实现它的类必须实现哪些方法。接口可以理解为一种协议，或者说标准，它只声明实现哪些函数，而这些函数功能的具体实现，交给实现（implements）它的类来完成。

+ 实现多个接口时，接口中的方法不能有重名。

+ 接口也可以继承，通过 extends 操作符。

+ 类要实现接口，必须使用和接口中所定义的方法完全一致的方式，否则会导致致命错误。

+ 接口中的方法必须都是共有，这是接口的特性。接口中的方法也都是抽象方法（抽象方法即只有声明没有实现的方法）。

+ 一个类只能有一个父类，但是一个类可以实现多个接口。

+ 一个类可以实现多个接口，也可以在继承一个类的同时实现多个接口，但是一定要先继承类再实现接口（指的是类声明时的顺序）。

#### 常量

接口中不能声明变量，但是可以定义常量，接口常量和类常量的使用完全相同，但是不能被子类或子接口所覆盖。

#### 示例

```php
<?php

interface A
{
    public function smile();

    public function pay($merchant, $money);

    //函数的类型增加了类型约束
    public function call(Person $person, int $phoneNumber);
}

class TestA implements A
{
    public function smile()
    {
        echo '^_^';
    }

    public function pay($merchant, $money)
    {
        echo 'Pay'.$money.'to'.$merchat->name;
    }

    public function call(Person $person, int $number)
    {
        echo 'calling'.$person.'</br>';
        echo 'calling'.$number;
    }
}

```

>上面的例子，TestA 类正确地实现了接口 A，接口中的每个方法都在类中予以实现。


```php
<?php

interface A
{
    public function foo();
}

interface B
{
    public function bar();
}

interface C extends A, B
{
    public function baz();
}

class D implements C
{
    public function foo()
    {
        echo __FUNCTION__;
    }

    public function bar()
    {
        echo __FUNCTION__;
    }

    public function baz()
    {
        echo __FUNCTION__;
    }
}
```

>上面代码中，C 接口继承了 A 和 B 两个接口，D 类实现了该接口。

```php
<?php

interface Foo
{
    const NAME = 'Kevin Garnett';
    function walk();
}

class Bar implements Foo
{
    function walk()
    {
        echo 'walk to school';
    }
}

class Baz implements Foo
{
    const NAME = 'Paul Pierce'; //错误，不能覆盖接口中定义的常量

    function walk()
    {
        echo 'walk back home';
    }
}

echo Bar::NAME;

echo Foo::NAME;
```
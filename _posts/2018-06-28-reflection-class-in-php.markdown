---
layout:     post
title:      "PHP 中的反射机制"
subtitle:   "Reflection Class in PHP"
date:       2018-06-28 21:00:00
author:     "ywlvs"
header-img: "img/post-bg-smartphone-wallpapers.jpg"
catalog: true
tags:
    - PHP
    - Reflection
---

反射，就是在程序运行的过程中动态的分析类或对象的状态，导出或提取出关于类、方法、属性和参数等信息，也包括代码中的注释。反射是操纵面向对象泛型中元模型的 API，可用于构建复杂、可扩展的应用。反射主要用于偏底层的实现中，在业务代码中并不常用，比如框架底层的服务容器等。Laravel 中的服务容器就使用了反射技术。

最常用的 API 是 ReflectionClass 和 ReflectionMethod。

---

#### ReflectionClass

> 这个接口可以用来获取类的信息。

定义一个简单的类

```php
<?php

class Goods
{
    public $price;

    public function __construct($price)
    {
        $this->price = $price;
    }

    public function getPrice()
    {
        echo $this->price;
    }
} 

```

使用反射类 ReflectionClass 来分析该类。

```php

$refClass = new ReflectionClass(Goods::class);
var_dump($refClass->getMethods());

```

运行该代码打印出类中包含的方法

```php

array(2) {
  [0]=>
  object(ReflectionMethod)#2 (2) {
    ["name"]=>
    string(11) "__construct"
    ["class"]=>
    string(5) "Goods"
  }
  [1]=>
  object(ReflectionMethod)#3 (2) {
    ["name"]=>
    string(8) "getPrice"
    ["class"]=>
    string(5) "Goods"
  }
}

```

可以看到，返回的结果是 ReflectionMethod 的数组，数组的每个元素是该类的方法信息。

其他的常用方法

```php

ReflectionClass::getMethods     //获取方法的数组
ReflectionClass::getName        //获取类名
ReflectionClass::hasMethod      //检查方法是否已定义
ReflectionClass::hasProperty    //检查属性是否已定义
ReflectionClass::isAbstract     //检查类是否是抽象类（abstract）
ReflectionClass::isFinal        //检查类是否声明为 final
ReflectionClass::isInstantiable //检查类是否可实例化
ReflectionClass::newInstance    //从指定的参数创建一个新的类实例

```

用上面的类做一个简单的测试。

```php

$refClass->getName();               // Goods
$refClass->hasMethod('getPrice');   // true
$refClass->hasProperty('price');    // true
$refClass->isAbstract();            //false
$refClass->isFinal();               //false
$refClass->isInstantiable();        //true

$newGoods = $refClass->newInstance();
$newGoods->getPrice();                    //输出 10.0

```

#### ReflectionMethod

主要是对方法的反射。

```php

<?php

class Greet
{
    public function say($message)
    {
        echo $message;
    }
}

$greeting = new Greet();
$refClass = new ReflectionClass(Greet::class);

$method = $refClass->getMethod('say');
$method->invoke($greeting, 'Good Morning');     //输出 Good Morning

```

常用的一些方法

```php

ReflectionMethod::invoke        // 执行
ReflectionMethod::invokeArgs    // 带参数执行
ReflectionMethod::isAbstract    // 判断方法是否是抽象方法
ReflectionMethod::isConstructor // 判断方法是否是构造方法
ReflectionMethod::isDestructor  // 判断方法是否是析构方法
ReflectionMethod::isFinal       // 判断方法是否定义 final
ReflectionMethod::isPrivate     // 判断方法是否是私有方法
ReflectionMethod::isProtected   // 判断方法是否是保护方法 (protected)
ReflectionMethod::isPublic      // 判断方法是否是公开方法
ReflectionMethod::isStatic      // 判断方法是否是静态方法
ReflectionMethod::setAccessible // 设置方法是否访问

```

#### 私有方法和私有属性

使用反射既可以访问私有属性，也可以执行私有方法。对于某些引用的外部 SDK，如果想使用某些类中的私有方法或者私有属性却无法使用，可以使用反射的方式实现。

下面尝试一个简单示例。

```php

<?php

class Secret
{
    private $secret;

    public $key;

    public function __construct($key)
    {
        $this->$key = $key;
        $this->secret = md5($key);
    }

    private function showSecret()
    {
        return $this->secret;
    }
}

$item = new Secret('nothing-is-impossible');
$refClass = new ReflectionClass($item);
$method = $refClass->getMethod('showSecret');
$method->setAccessible(true);
echo $method->invoke($item);    // 输出f6ded05a949bf62753b2f19d8d401769

$property = $refClass->getProperty('secret');
$property->setAccessible(true);
echo $property->getValue($item); // 输出f6ded05a949bf62753b2f19d8d401769

```

上面的例子中，分别调用了类的私有方法和私有属性，注意，上面有使用 setAccessible 这个函数，如果不先调用该函数，是无法实现对私有属性以及私有方法的访问的。


---

##### 参考文献

+ [反射在 PHP 中的应用](https://laravel-china.org/articles/7538/the-application-of-reflection-in-php)
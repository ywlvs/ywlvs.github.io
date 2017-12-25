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

### 学习 PHP，有些基础知识是必须要补一补的，今天记录一下 php 中魔术方法和魔术常量的一些知识，原文来自 [严肃的博客](http://yansu.org/2014/04/27/magic-methods-and-magic-constants-in-php.html)。

---

#### 魔术方法（Magic methods）

>PHP 中把以两个下划线 __ 开头的方法称为魔术方法，这些方法不需要显式的调用，而是由某种特定的条件出发。

+ `__construct()`，类的构造函数
+ `__destruct()`，类的析构函数
+ `__call()`，在对象中调用一个不可访问方法时调用该函数
+ `__callStatic()`，用静态方式调用类中一个不可访问方法时调用
+ `__get()`，获取类中某属性时调用该函数
+ `__set()`，设置类中的属性时调用该函数
+ `__isset()`，当对不可访问属性调用 `isset()` 或 `empty()` 时调用
+ `__unset()`，当对不可访问属性调用 `unset()` 时调用
+ `__sleep()`，执行 `serialize()` 时，会先调用这个函数
+ `__wakeup()`，执行 `unserialize()` 时，会先调用这个函数
+ `__toString()`，类被当成字符串处理时的回应方法
+ `__invoke()`，调用函数的方式调用一个对象时的回应方法
+ `__set_state()`，调用 `var_export()` 导出类时，此静态方法会被调用
+ `__clone()`，当对象复制完成时调用

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

?>

```

这两个方法在继承时可以扩展，例如：

```php

<?php

class TmpFileRead extends FileRead
{
    function __construct()
    {
        parent::__construct();
    }

    function __destruct()
    {
        parent::__destruct();
    }
}

?>

```

**`__call()` 和 `__callStatic()`**

> 在对象中调用一个不可访问的方法时，会调用这两个方法，其中后者为静态方法。不可访问的含义，包括用 protected 和 private 关键字声明的函数，也包括未定义的函数。

```php

<?php

class MethodTest
{
    public function __call($name, $args)
    {
        echo 'Calling object method' . $name . implode(',', $args) . "\n";
    }

    public static function __callStatic($name, $args)
    {
        echo 'Calling static method' . $name . implode(',', $args) . "\n";
    }
}

$obj    = new MethodTest;
$obj->runTest('in object context');

MethodTest::runTest('in object context');

?>

```

**`__get()` , `__set()` , `__isset()` 和 `__unset()`**

>操作一个类的属性时调用该函数

```php

<?php

class MethodTest
{
    private $data = [];

    public function __set($name, $value)
    {
        $this->data[$name]  = $value;
    }

    public function __get($name)
    {
        if (array_key_exists($name, $this->data)) {
            return $this->data[$name];
        }

        return null;
    }

    public function __isset($name)
    {
        return isset($this->data[$name]);
    }

    public function unset($name)
    {
        unset($this->data[$name]);
    }
}

?>

```

**`__sleep()` 和 `__wakeup()`**

>当执行 `serialize()` 和 `unserialize()` 时， 会先调用 `__sleep()` 和 `__wakeup()` 函数。

```php

<?php

class Connection
{
    protected $connection;

    private $server, $userName, $pwd, $db;

    public function __construct($server, $name, $pwd, $db)
    {
        $this->server   = $server;
        $this->userName = $name;
        $this->pwd      = $pwd;
        $this->db       = $db;

        $this->connect();
    }

    private function connect()
    {
        $this->connection = mysql_connect($this->server, $this->userName, $this->pwd);
        mysql_select_db($this->db, $this->connection);
    }

    public function __sleep()
    {
        return ['server', 'userName', 'pwd', 'db'];
    }

    public function __wakeup()
    {
        $this->connect();
    }
}

?>

```

**`__toString()`**

> 当对象需要被当做字符串使用时的回应方法。 例如使用 `echo $obj;` 来输出一个对象。这个方法只能返回字符串，并且不可以在这个方法中抛出异常，否则会出现致命错误。

```php

<?php

class SimpleClass
{
    public function __toString()
    {
        return 'this is an object';
    }
}

$obj = new SimpleCalss;
echo $obj;

?>

```

**`__invoke()` 方法**

>调用函数的方式调用一个对象时的回应方法。

```php

<?php

class CallableClass
{
    function __invoke()
    {
        echo 'this is an object';
    }
}

$obj = new CallableClass;
var_dump(is_callable($obj));

?>

```

**`__set_state()`**

>调用 `var_export()` 导出类时，此静态方法会被调用。

```php

<?php

class A
{
    public $var1;
    public $var2;

    public static function __set_state($array)
    {
        $obj = new A;
        $obj->var1 = $array['var1'];
        $obj->var2 = $array['var2'];

        return $obj;
    }
}

$a = new A;
$a->var1 = 5;
$a->var2 = 'foo';

var_dump(var_export($a));

?>

```

**`__clone()`**

>当对象完成复制时调用此函数。

```php

<?php

class Singleton
{
    private static $_instance = null;

    private function __construct() {}

    public static function getInstance()
    {
        if (is_null(self::$_instance)) {
            self::$_instance = new Singleton();
        }

        return self::$_instance;
    }

    /**
     * 防止克隆实例
     */
    public function __clone()
    {
        die('Clone is not allowed.' . E_USER_ERROR);
    }
}

?>

```

#### 魔术常量（Magic constants）

>PHP 中的常量大部分都是不变的， 但是有8个常量会随着他们所在代码位置的变化而变化，这8个常量被称为魔术常量。这些魔术常量常常被用户获得当前环境信息或者记录日志。

+ `__LINE__`，文件中的当前行号
+ `__FILE__`，文件的完整路径和文件名
+ `__DIR__`，文件所在的目录
+ `__FUNCTION__`，函数的名称
+ `__CLASS__`，类的名称
+ `__TRAIT__`，Trait 的名称
+ `__METHOD__`，类的方法名
+ `__NAMESPACE__`，当前命名空间的名称
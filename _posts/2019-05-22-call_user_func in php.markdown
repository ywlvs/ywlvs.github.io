---
layout:     post
title:      "PHP 中的两个函数：call_user_func 和 call_user_func_array"
subtitle:   "call_user_func and call_user_func_array in PHP"
date:       2019-05-23 10:02:00
author:     "ywlvs"
header-img: "img/post-bg-sequences.png"
catalog: true
tags:
    - PHP
---


# call_user_func

该函数将传入的第一个参数当作回调函数来调用。其余的参数作为回调函数的参数。

```php
function schedule($begin, $end)
{
    echo 'begin-time: ', $begin, "\n", 'end-time: ', $end;
}

call_user_func('schedule', 12, 13);
```

上述例子会输出

```
begin-time: 12
end-time: 13
```

# call_user_func_array

与 call_user_func 函数作用类似，但是回调函数的参数是以数组的形式传入的。

对于上面的例子，使用 call_user_func_array 来实现函数的调用。

```php
call_user_func_array('schedule', [12, 13]);
```

相比 call_user_func 函数，使用数组传递回调函数的参数的这种形式，更加直观。

## 注意事项

传入 call_user_func 的参数，不能为引用传递。

```php
error_reporting(E_ALL);

function change(&$name)
{
    $name = 'Tom';
}

$name = 'Jerry';

call_user_func('change', $name);

echo $name, "\n";
```

上面例子中，change 函数接收变量的引用作为形参。若使用 call_user_func 调用 change 函数，则无法传递引用。在 E_ALL 的错误级别下，会产生一个警告。

```
PHP Warning:  Parameter 1 to change() expected to be a reference, value given in index.php on line 22
```

## 调用类中的方法

```php
class A
{
    public function __construct()
    {
        echo 'class is being created';
    }

    public function morning()
    {
        echo 'good morning', "\n";
    }

    static function good()
    {
        echo 'God bless you', "\n";
    }
}

call_user_func(['A', 'good']);
call_user_func(['A', 'morning']);

call_user_func('A::good');
call_user_func('A::morning');
```

上面的例子输出的结果为：

```
God bless you
good morning
God bless you
good morning
```
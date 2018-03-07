---
layout:     post
title:      "装饰模式的学习与理解"
subtitle:   "Some Thinking about Decorator Pattern"
date:       2018-03-07 10:10:00
author:     "ywlvs"
header-img: "img/post-bg-two-wolves-coming.jpg"
catalog: true
tags:
    - PHP
    - Middleware
    - Decorator Pattern
---

先贴一段代码，代码源自于《Laravel 框架关键技术解析》。

```php
<?php

interface Decorator
{
    public function display();
}

class XiaoFang implements Decorator
{
    private $name;

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function display()
    {
        echo '我是' . $this->name . '我出门了！！！' . '</br>';
    }
}

class Finery implements Decorator
{
    private $component;

    public function __construct(Decorator $component)
    {
        $this->component = $component;
    }

    public function display()
    {
        $this->component->display();
    }
}

class Shoes extends Finery
{
    public function display()
    {
        echo '穿上鞋子' . '<br>';

        parent::display();
    }
}

class Skirt extends Finery
{
    public function display()
    {
        echo '穿上裙子' . '<br>';

        parent::display();
    }
}


class Fire extends Finery
{
    public function display()
    {
        echo '出门前先整理头发' . '<br>';

        parent::display();

        echo '出门后再整理一下头发' . '<br>';
    }
}

$xiaofang = new XiaoFang('小芳');
$shoes = new Shoes($xiaofang);
$skirt = new Skirt($shoes);
$fire = new Fire($skirt);
$fire->display();

```

装饰模式指的是在不必改变原类文件和使用继承的情况下，动态地扩展一个对象的功能。它是通过创建一个包装对象，也就是装饰来包裹真实的对象。

上面例子中，XiaoFang 就是需要被装饰的对象，Finery 就是用于包装的对象，它们都实现了 Decorator。而 Skirt 和 Shoes 均继承了 Finery，也就是用于包装的实物。最终的结果就是，小芳精心打扮之后，快乐的出门去了。更通俗一点讲，小芳要出门，出门前要穿裙子以及鞋子，至于是先穿鞋子还是先穿裙子，这不是很重要，因为在该例中，裙子和鞋子都属于小芳的包装品。如果还想给小芳戴个发卡，那再构造一个发卡类就好了。

在 Laravel 框架中，装饰模式最典型的使用就是“中间件”，请求经由中间件的处理（装饰），最终到达控制器，嗯，这里说的是前置控制器。
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

>先贴一段代码，代码源自于《Laravel 框架关键技术解析》。

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
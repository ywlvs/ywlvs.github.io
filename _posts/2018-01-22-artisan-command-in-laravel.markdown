---
layout:     post
title:      "Laravel 中常见的 artisan 命令"
subtitle:   "Artisan Commands in Laravel"
date:       2018-01-22 22:03:00
author:     "ywlvs"
header-img: "img/post-bg-the-world-of-web-characters.jpg"
catalog: true
tags:
    - Linux
    - Laravel
---

>使用的版本是 Laravel 5.5

+ php artisan make:post Post -m

使用 `-m` 选项可以同时生成对应的 migration 文件

+ php artisan make:factory PostFactory --model=Post

使用 `--model` 选项可以指定相应的模型

`tinker`

>tinker 是一个命令行脚本工具，可以通过使用 ORM 的语法在命令行工具当中与数据库进行交互

php artisan tinker

启动并进入 tinker 的交互界面

在 tinker 中，可以使用 factory 方法

factory(App\Post)->make()

该命令会生成一条 Post 的实例，注意，这里使用的是 make 方法，实例会生成，但是不会保存到数据库中。

factory(App\Post, 10)->create()

这条命令表示生成 10 条 Post 实例数据并写入数据库中

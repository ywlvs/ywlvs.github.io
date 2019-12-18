---
layout:     post
title:      "MySQL 中的存储过程"
subtitle:   "MySQL Stored Procedure"
date:       2019-12-12 09:16:00
author:     "ywlvs"
header-img: "img/post-bg-stored-procedure.jpg"
catalog: true
tags:
    - MySQL
    - Procedure
---

目前对存储过程还没有深入的了解，仅仅记录一下一些可能用到的操作。

## 查看定义了哪些存储过程

+ 使用 show 命令

```
show procedure status;
```

可以使用 `like` 和 `where` 关键字对结果进行过滤

```
show procedure status like '%procedure-name%';
show procedure status where db = 'db-name';
```

+ 查找 mysql.proc

```
select * from `mysql`.`proc` where `db` = 'my_database_name' and `type` = 'PROCEDURE';
```

+ 查找 information.routines

```
select * from information.routines where routine_type = 'PROCEDURE' and routine_schema = 'your-db-name';
```

## 查看存储过程的详细定义

```
show create procedure db_name.procedure_name;
```

## 存储过程的修改

只能修改存储过程的特征，不能修改存储过程的参数和过程体。如果需要修改过程体或者参数，先使用 drop 命令删掉该存储过程，再重新创建。

## 一个简单的存储过程


```
declare i int;
set i = 1;
while(i <= 100000) do
insert into t values(i,i,i);
set i = i + 1;
end while;
end

```

源于-MySQL 技术内幕 SQL 编程

```
create table nums(id int not null primary key);
delimiter $$
create procedure pCreateNums(cnt int)
begin
declare s int default 1;
truncate table nums;
while s<=cnt do
insert into nums select s;
set s=s+1;
end while;
end $$
delimiter ;
delimiter $$
create procedure pFastCreateNums(cnt int)
begin
declare s int default 1;
truncate table nums;
insert into nums select s;
while s*2<=cnt do
insert into nums select id+s from nums;
set s=s*2;
end while;
end $$
delimiter ;
```
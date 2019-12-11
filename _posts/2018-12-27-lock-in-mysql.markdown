---
layout:     post
title:      "MySQL 中的锁"
subtitle:   "Lock in MySQL"
date:       2018-12-28 16:00:00
author:     "ywlvs"
header-img: "img/post-bg-mysql-lock.jpg"
catalog: true
tags:
    - MySQL
---

从锁的作用范围上来划分，MySQL 中的锁有三个级别：

+ 全局锁
+ 表级锁
+ 行级锁

## 全局锁

全局锁，指的是为整个数据库实例加上只读锁，此时，整个数据库处于只读状态。加全局锁的命令是 `FLUSH TABLES WITH READ LOCK(FTWRL)`，执行该命令后，其它线程的以下语句会被阻塞：数据更新语句（数据的增删更），数据定义语句（包括建表和更改表结构等）以及更新类事务的提交语句。

全局读锁的典型应用场景是，做整库的逻辑备份，就是把整个库都 select 出来保存成文本。

全库只读，有一定的风险，比如：

+ 在主库中执行备份，那么主库的数据就无法更新，就会造成业务的停摆

+ 在从库中进行备份，则从库的数据无法及时更新，就会造成主从不一致

### 使用 mysqldump 进行备份

不加全局锁就对全库进行备份，得到的库不是一个逻辑时间点，这个视图是逻辑不一致的。加锁备份会影响应用的正常业务，那么，究竟有没有一种可以保证业务正常并且数据逻辑一致的备份方案呢？答案是，有，但是是有条件的。

MySQL 提供了一个叫 `mysqldump` 的备份工具，使用该工具进行数据备份的时候，加上 `-single-transaction` 的参数，会在导出备份数据之前开启一个事务，在**可重复读**的隔离级别下，可以保证其他线程正常的数据更新，并且备份数据是逻辑一致的。

然而，并不是所有的存储引擎都支持该隔离级别，比如 MyISAM 存储引擎就不支持事务，就无法使用该方法保证备份时数据的一致性。所以，`single-transaction` 方式只适用于**所有的**表使用事务引擎的库。

### 设置全局只读

不使用全局读锁也可以使全库进入只读状态，`set global readonly = true`，但是，使用该方法也有一些不足。

+ 业务中，全局 readonly 的值可能会有其他的作用，比如判断该库使主库还是从库，所以，全局修改 readonly 的值，在业务上有一定的风险

+ 从实现机制上来说，使用 FTWRL 的方式加全局锁之后，如果客户端意外断开，则该锁会自动释放，不会影响其它线程的业务。然而，全局修改 readonly 的值，如果客户端意外断开，数据库将长时间处于只读的状态，对业务的影响会更大。

## 表级锁

表级锁有两种，一种是表锁，一种是元数据锁（Meta Data Lock）。

### 表锁

加表锁的命令是，`lock tables ... read/write`，与 FTWRL 类似，可以使用 unlock 来释放表锁，也可以在客户端断开连接的时候自动释放表锁。需要注意的是，**使用 lock tables 语法，不仅会限制其它进程对数据的操作，同事也会限制当前进程对数据的操作** 。

如 A 线程执行了 `lock tables t1 read, t2 write`，在该锁被释放前，A 线程能进行的操作包括对 t1 表的读以及对 t2 表的读和写，无法操作其它表，其它线程 写 t1，读写 t2 的操作都会被阻塞。

### MDL-元数据锁

MDL 锁从 MySQL5.5 开始引入，用以保证独写的正确性。

MDL 锁无需显示使用，当访问一个表的时候会自动加上。当对一个表进行增删该查时，加 MDL 读锁；当进行表结构的修改时，加 MDL 写锁。

+ 读锁之间不互斥，因此可以有多个线程同时对一个表进行增删改查的操作

+ 读锁和写锁，写锁和写锁之间是互斥的，用来保证变更表结构操作的安全性。因此，如果同时有两个线程要修改一个表的结构，其中一个线程要等到另一个线程完成之后才可以。

MDL 锁是在访问表时系统自动加上的，但是却不可忽视。如果一个事务 A 长时间不释放 MDL 读锁，由于 MDL 读锁和写锁互斥，这时，若某一线程 M 想要修改表结构，M 就只能等待，那么在 M 之后的各种操作就只能排队等候，指定时间内如果失败，客户端如果有重试机制，很快，这个库的连接就会爆满。

```
ALTER TABLE tbl_name NOWAIT add column ...
ALTER TABLE tbl_name WAIT n add column ...
```

修改表的时候，可以在语句中加上等待时间的语句，如果在指定时间内没有拿到 MDL 写锁，则放行后面的语句，不耽误业务语句的执行。

## 几个锁的例子

+ MySQL Version:5.7.27

+ transaction_isolation:REPEATABLE-READ
+ innodb_deadlock_detect:ON
+ innodb_lock_wait_timeout:50

先创建一个表，并插入若干条测试数据。

```sql
CREATE TABLE `room_area` (
  `number` varchar(255) DEFAULT NULL,
  `area` float DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8

insert into room_area (`number`, 'area') values ('C1211', 35), ('C1212', 25), ('C1213', 35), ('C1214', 42), ('C1215', 25), ('C1216', 20), ('C1217', 20), ('C1218', 18), ('C1219', 18), ('C1220', 18), ('C1221', 18), ('C1222', 18), ('C1301', 28), ('C1302', 55), ('C1303', 25), ('C1304', 25), ('C1305', 18), ('C1306', 18), ('C1307', 21), ('C1308', 22), ('C1309', 23);
```

目前表中的数据具体数据如下：

|**number**|**area**|
|:--:|:--:|
| C1211  |   35 |
| C1212  |   25 |
| C1213  |   35 |
| C1214  |   42 |
| C1215  |   25 |
| C1216  |   20 |
| C1217  |   20 |
| C1218  |   18 |
| C1219  |   18 |
| C1220  |   18 |
| C1221  |   18 |
| C1222  |   18 |
| C1301  |   28 |
| C1302  |   55 |
| C1303  |   25 |
| C1304  |   25 |
| C1305  |   18 |
| C1306  |   18 |
| C1307  |   21 |
| C1308  |   22 |
| C1309  |   23 |

需要注意的是，创建该表的时候，**未定义主键**字段，同时**未创建任何索引**。

### 场景一

|t|session A|session B|
|:--:|:--:|:--:|
|t1|start transaction with consistent snapshot;|start transaction with consistent snapshot;|
|t2|update room_area set area = 24 where number = 'C1309';||
|t3||update room_area set area = 24 where number = 'C1308';|

此场景中，t2 时刻（session A）中的更新语句正常执行，t3 时刻（session B）中语句被阻塞，超时之后会有如下提示：

```
 Lock wait timeout exceeded; try restarting transaction
```

### 场景二

给 room_area 在 number 字段上创建索引。

```sql
alter table room_area add index idx_number(`number`);
```

|t|session A|session B|
|:--:|:--:|:--:|
|t1|start transaction with consistent snapshot;|start transaction with consistent snapshot;|
|t2|update room_area set area = 24 where number = 'C1309';||
|t3||update room_area set area = 25 where number = 'C1308';|
|t4|update room_area set area = 26 where number = 'C1308';|#blocked|
|t5||update room_area set area = 27 where number = 'C1309';#blocked|

t2 时刻（session A）与 t3 时刻（session B）的语句均顺利执行。t4 时刻（session A）的语句被 blocked 住，处于等待状态，t5 时刻（session B）的语句也被 blocked 住，不会执行，但是此时触发了死锁检测，由于是 session B 在 t5 时刻的这条语句是后发起的，所以该语句的执行被放弃。之后 t4 时刻（session A）的语句执行完成。

t5 时刻语句被放弃执行时，抛出的异常提示如下：

```
Deadlock found when trying to get lock; try restarting transaction
```
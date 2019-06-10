---
title: 杂谈MySQL - 基础篇
categories:
- 数据库
date: 2017-08-12 
tags:
- MySQL
---
* `CHAR 与 VARCHAR`
>CHAR以固定长度的分配存储空间。
VARCHAR根据实际存储的数据来分配最终的存储空间。
VARCHAR类型的实际长度是它的值的实际长度+1。
CHAR(M)定义的列的长度为固定的，M取值可以为0～255之间。
VARCHAR(M)定义的列的长度为可变长字符串，M取值可以为0~65535之间。
CHAR当字符位数不足时，系统并不会采用空格来填充。
CHAR和VARCHAR存储的内容超出设置的长度时，内容会被截断。
VARCHAR(50)和(200)存储hello所占空间一样，但后者在排序时会消耗更多内存。

* `TRUNCATE 与 DELETE`
>DELETE命令删除的数据将存储在系统回滚段中，需要的时候，数据可以回滚恢复。
TRUNCATE命令删除的数据是不可以恢复的。
TRUNCATE TABLE是非常快的。
TRUNCATE之后的自增字段从头开始计数了，而DELETE的仍保留原来的最大数值。

* `float double 与 decimal`
>float，double非标准类型，在DB中保存的是近似值。
>Decimal则以字符串的形式保存数值。
>小数类型为 decimal，禁止使用 float 和 double。
>存储商品价格可以使用分作为单位，存储整数。

* `InnoDB 与 MyISAM`
>InnoDB支持事务，MyISAM不支持；
InnoDB数据存储在共享表空间，MyISAM数据存储在文件中；
InnoDB支持行级锁，MyISAM只支持表锁；
InnoDB支持崩溃后的恢复，MyISAM不支持；
InnoDB支持外键，MyISAM不支持；
InnoDB不支持全文索引，MyISAM支持全文索引；

* `共享锁(读锁) 与 排他锁(写锁)`
>共享锁又称读锁（在查询语句后面增加LOCK IN SHARE MODE），是读取操作创建的锁。其他用户可以并发读取数据，但任何事务都不能对数据进行修改（获取数据上的排他锁），直到已释放所有共享锁。
>排他锁又称写锁（在查询语句后面增加FOR UPDATE），如果事务T对数据A加上排他锁后，则其他事务不能再对A加任任何类型的封锁。获准排他锁的事务既能读数据，又能修改数据。

* `表锁 与 行锁`
>行锁的劣势：开销大，加锁慢，会出现死锁。
>行锁的优势：锁的粒度小，发生锁冲突的概率低，处理并发的能力强。
>表锁的优势：开销小；加锁快；无死锁 
>表锁的劣势：锁粒度大，发生锁冲突的概率高，并发处理能力低。

* `索引的类型`
>普通索引：最基本的索引，没有任何约束限制。
唯一索引：和普通索引类似，但是具有唯一性约束。
主键索引：特殊的唯一索引，不允许有空值。

* `索引的创建原则`
>创建索引的列是出现在WHERE或ON子句中的列。
>索引列的基数越大，数据区分度越高，索引的效果越好。
>对于字符串进行索引，应该制定一个前缀长度，可以节省大量的索引空间。
>避免创建过多的索引，索引会额外占用磁盘空间，降低写操作效率。

* `索引的前缀原则`
```
KEY(a,b,c)
WHERE a = 1 AND b = 2 AND c = 3
WHERE a = 1 AND b = 2
WHERE a = 1
#以上SQL语句可以用到索引
WHERE b = 2 AND c = 3
WHERE a = 1 AND c = 3
#以上SQL语句用不到索引
```
* `关联查询`
>内连接（INNER JOIN），外连接（LEFT/RIGHT JOIN）
>INNER JOIN可以缩写为JOIN

* `联合查询`
>UNION与UNION ALL
>UNION ALL，不会合并重复的记录行
>UNION 效率高于 UNION ALL






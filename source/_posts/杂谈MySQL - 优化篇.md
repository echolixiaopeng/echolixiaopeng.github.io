---
title: 杂谈MySQL - 优化篇
categories:
- 数据库
date: 2018-04-14 
tags:
- MySQL
---
* `show processlist`
>显示哪些线程正在运行。
>id       #ID标识，要kill一个语句的时候很有用
use      #当前连接用户
host     #显示这个连接从哪个ip的哪个端口上发出
db       #数据库名
command  #连接状态，一般是休眠（sleep），查询（query），连接（connect）
time     #连接持续时间，单位是秒
state    #显示当前sql语句的状态
info     #显示这个sql语句

* `explain`
>查询SQL的执行计划。
>possible_keys       #字段上存在的索引
>key                 #实际决定使用的索引
>rows              #估算的找到所需的记录所需要读取的行数

* `索引的优化`
>LIKE查询，%不能在前，否则用不到索引。
>列类型是字符串，查询时一定要给值加引号，否则索引失效。
>确保GROUP BY和ORDER BY只有一个表中的列，这样MySQL才有可能使用索引。
>is null将导致引擎放弃使用索引。
>使用!=或<>操作符，否则引擎将放弃使用索引。
>使用or 来连接条件，否则将导致引擎放弃使用索引。
>in 和 not in ，会导致全表扫描。
>where子句中对字段进行表达式操作，将导致引擎放弃使用索引。
>where子句中对字段进行函数操作，将导致引擎放弃使用索引。

* `LIMIT优化`
>LIMIT偏移量大的时候，查询效率较低。
可以记录上次查询的最大ID，下次查询时直接根据该ID来查询。

* `随机查询优化`
```
SELECT * FROM users 
WHERE 
id >= 
((SELECT MAX(id) FROM users)-(SELECT MIN(id) FROM users)) 
* 
RAND() + (SELECT MIN(id) FROM users)
LIMIT 10
```

* `count优化`
>COUNT(1)和COUNT(\*)性能相似
>count(列名)性能差

* `分区表`
>分区表是一个独立的逻辑表，但是底层MySQL将其分成了多个物理子表。
>执行查询时，优化器会根据分区定义，只需要查询数据所在分区。
>所有分区都必须使用相同的存储引擎。
>一个表最多只能有1024个分区。

* `主从复制`
>主库上把二进制日志，复制到从库，从库重放日志。
>降低单个服务器的压力。
>避免单点失败。




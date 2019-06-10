---
title: Hbase初识
categories:
- 数据库
date: 2017-08-28 
tags:
- Hbase
---
>开源的分布式数据库
>基于Google的BigTable论文设计
>面向列存储
>构建于HDFS之上
>可以快速随机查询海量的结构化数据
>No Schema

#### 为什么快
`写入快`
HBase的写入速度快是因为它其实并不是真的立即写入文件中，而是先写入内存，随后异步刷入HFile。所以在客户端看来，写入速度很快。另外，写入时候将随机写入转换成顺序写，数据写入速度也很稳定。

`读取快`
客户端可以直接定位到要查数据所在的HRegion server服务器，然后直接在服务器的一个region上查找要匹配的数据，并且这些数据部分是经过cache缓存的。

#### 数据模型
  `RowKey`
  是表中每条记录的“主键”，方便快速查找，Rowkey的设计非常重要。
 `Column Family`
  列族，拥有一个名称(string)，包含一个或者多个相关列
 `Column`
  属于某一个columnfamily，familyName:columnName，每条记录可动态添加
 `Version Number`
  类型为Long，默认值是系统时间戳，可由用户自定义
 `Value(Cell)`
  Byte array

#### 集群角色

`zookeeper`
1.保证任何时候，集群中只有一个master
2.存贮所有Region的寻址入口
3.实时监控Region Server的状态，将Region server的上线和下线信息实时通知给Master
4.存储Hbase的schema，包括有哪些table，每个table有哪些column family
 
`master`
1.为Region server分配region
2.负责region server的负载均衡
3.发现失效的region server并重新分配其上的region
4.GFS上的垃圾回收
5.处理schema更新请求
 
`Region server`
1.Region server 维护Master分配给它的region，处理对这些region的IO请求
2.Region server 负责切分在运行过程中变得过大的region
 
可以看到，client访问hbase上数据的过程并不需要master参与
（寻址访问zookeeper和region server，数据读写访问region server）,
master仅仅维护着table和region的元数据信息，负载很低


#### Rowkey设计
`长度原则`
越短越好，设计成定长。

`散列原则`
提高数据均衡分布在每个RegionServer，避免热点现象。

`唯一原则`
保证唯一



---
title: Redis必知必会
categories:
- 数据库
date: 2018-05-02 
tags:
- Redis
---
>开源KV数据库
>由C语言编写
>支持丰富的数据类型：string，set，list，sorted set，hash
>数据操作具有原子性
>支持数据持久化
>可集群扩展 

#### 优势

- 速度快，整个数据库统统加载在内存当中进行操作；
- 支持保存多种数据结构；
- 操作都是原子性；
- 支持事务；
- 丰富的特性，支持有序集合，支持ttl，支持发布订阅；

#### 数据结构

`string`
- 赋值：SET key value。如set hello world
- 取值：GET key。如get hello。返回是world
- 自增：INCR key。该key的值都会+1。该操作是原子操作。
- 自减：DECR key。该key的值都会-1。该操作是原子操作。

`list`
- 向头部插入：LPUSH key value1 value2...
- 向尾部插入：RPUSH key value1 value2...
- 从头部弹出：LPOP key。返回被弹出的元素值。
- 从尾部弹出：RPOP key。返回被弹出的元素值。
- 列表元素个数：LLEN key。key不存在返回0。
- 获取列表的子列表：LRANGE start end。
            
`set`
- 增加：SADD key value。
- 删除：SREM key value。
- 获取指定集合的所有元素：SMEMBERS key。
- 判断某个元素是否存在：SISMEMBER key value。
- 差集运算：SDIFF key1 key2...。对多个集合进行差集运算。
- 交集运算：SINNER key1 key2...。对多个集合进行交集运算。
- 并集运算：SUNION key1 key2...。对多个集合进行并集运算。
- 获取集合中元素个数：SCARD key。返回集合中元素的总个数。
            
`sorted set`
- 增加：ZADD key sorce1 value1 sorce2 value2...。
- 获取分数：ZSCORE key value。
- 获取分数范围内的元素：ZRANGEBYSCORE key min max 
- 为某个元素增加分数：ZINCRBY key increment value。

`hash`
- 赋值：HSET key field value。
- 取值：HGET key field。
- 同一个key多个字段赋值：HMSET key field1 value1 field2 value2...。
- 同一个KEY多个字段取值：HMGET key field1 fields2...。
- 获取KEY的所有字段和所有值：HGETALL key。
- 字段是否存在：HEXISTS key field。存在返回1，不存在返回0。
当字段不存在时赋值：HSETNX key field value。如果key下面的字段field不存在，则建立field字段，且值为value。如果field字段存在，则不执行任何操作。这个命令的是原子操作。
            
#### 持久化

`RDB`
>#在900秒之后，如果至少有1个key发生变化，则dump内存快照
>save 900 1  
          

- RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。
- 性能最大化。对于Redis的服务进程而言，在开始持久化时，它唯一需要做的只是fork出子进程，之后再由子进程完成这些持久化的工作，这样就可以极大的避免服务进程执行IO操作了。
- 相比于AOF机制，如果数据集很大，RDB的启动效率会更高。
- 如果当数据集较大时，可能会导致整个服务器停止服务。

`AOF`
>appendfsync always     #每次有数据修改发生时都会写入AOF文件。
>appendfsync everysec  #每秒钟同步一次，该策略为AOF的缺省策略。
>appendfsync no          #从不同步。高效但是数据不会被持久化。

- AOF持久化以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。
- 该机制可以带来更高的数据安全性。
- Redis 4.0 推出RDB-AOF 混合持久化。

#### 内存淘汰
maxmemory参数，可以控制其最大可用内存大小（字节）。
超过maxmemory时，根据maxmemory-policy参数进行内存淘汰。
>volatile-lru     使用LRU算法删除一个键（只对设置了生存时间的键）
allkeys-lru       使用LRU算法删除一个键
volatile-random   随机删除一个键（只对设置了生存时间的键）
allkeys-random    随机删除一个键
volatile-ttl      删除生存时间最近的一个键
noeviction（默认配置）  不删除键，只返回错误

#### 集群扩展
`官方cluster方案`
配置成主从结构，即一个master主节点，挂n个slave从节点。如果主节点失效，redis cluster会根据选举算法从slave节点中选择一个上升为master节点，整个集群继续对外提供服务。

`twemproxy`
twitter开源的代理分区方案。

`codis`
豌豆荚开源的代理分区方案。

`客户端分片`
分区的逻辑在客户端实现，由客户端自己选择请求到哪个节点。

#### 注意

- keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。
- 使得过期时间分散一些，否则可能会出现短暂的卡顿现象。
---
title: MongoDB必知必会
categories:
- 数据库
date: 2018-04-03 
tags:
- MongoDB
---
>NoSQL文档数据库
>由C++语言编写
>支持分布式集群扩展

#### 优势

- 灵活，No Schema 
- 解决海量数据存储，并有良好的查询性能
- 快速，热数据加载到内存，不用join
- GEO支持，轻松解决附近位置查询场景

#### 常用命令
```
1）查看所有数据库（show dbs）

2）创建新的数据库（use mydb）

3）获取所有的集合（表）

db.getCollectionNames();

4）插入数据。

db.collName.insert({name: 'Kane', gender: 'male'});

5）更新数据

db.collName.update({_id: ObjectId('4df96d7fbc7a05156600e4f2')}, {$set: {name: 'Kane', gender: 'male',weight:111}});

6）删除数据

db.collName.remove({name: 'Kane'});

7）获取集合的文档（记录）数量。

db.collName.count();

8）获取集合的所有文档

db.collName.find();

9）获取集合中性别为male的文档

db.collName.find({gender:'male'});

10）获取集合中性别为男性且体重大于600的文档

db.collName.find({gender:'male',weight:{$gt:600}});

11）展示名字和体重的所有文档

db.collName.find(null,{name:1,weight:1});

12）找出性别为male，体重前2、3名的名字

db.collName.find({gender: 'male'}, {name: 1}).sort({weight:-1}).limit(2).skip(1);

13）计算体重小于600的数量

db.collName.count({weight:{$lt:600}});


```


#### GEO支持

```
Point location = new Point(-73.99171, 40.738868);
NearQuery query = NearQuery.near(location).maxDistance(new Distance(10, Metrics.MILES));

GeoResults<Restaurant> = operations.geoNear(query, Restaurant.class);
```


#### Object ID
ObjectId 是一个12字节 BSON 类型数据，有以下格式：
>前4个字节表示时间戳
接下来的3个字节是机器标识码
紧接的两个字节由进程id组成（PID）
最后三个字节是随机数。

为了减少服务器端的开销，objectId是由客户端的驱动程序来生成的。

#### 索引
* 类型

`单键索引`
db.collection.createIndex({'fieldName':1});

`唯一索引`
db.books.ensureIndex({name:-1}, {unique:true});

`复合索引`
db.collection.createIndex({'fieldName_one':1,'fieldName_n..':1});

`TTL索引`
db.collection.createIndex({'dateTimeField':1},{expireAfterSeconds:100(秒)});
在一段时间后会过期的索引，在索引过期后，相应的数据会被删除。

`地理空间索引`
 db.collection.createIndex({w:"2d"})

* 注意

`后台创建索引`
db.values.createIndex({open: 1}, {background: true})
建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引。
`查看查询计划`
db.books.find(query).explain();
分析查询耗时。

#### 集群
`模式`
分为三种模式：主从，副本，sharding

`副本模式`
三类角色：主节点，副节点，仲裁节点

---
title: 闲扯RESTful
categories:
- 方法论
date: 2017-11-12
tags:
- TDD
---
#### 什么是RESTful
>REST，即Representational State Transfer，表现层状态转化。
每一个URI代表一种资源。
http的method代表对资源的操作。
POST 创建，PUT 更新，GET 获取，DELETE 删除。


#### 为什么用RESTful
>无状态，不用考虑上下文。
面向资源，具有自解释性。


#### 局限性
`不是所有的请求都是资源`
例如订单的状态变更接口，难以使用RESTful去描述接口。

`批量删除`
DELETE /tickets/12 #单条删除ticekt 12
使用RESTful风格如何定义批量删除？

`批量查询URL长度问题`
RESTful要求使用get获取资源。
批量查询时，参数过长，url长度超过了get方法的url最大值。

`增加接口切面定义难度`
url路径参数，增加了接口切面的定义难度。
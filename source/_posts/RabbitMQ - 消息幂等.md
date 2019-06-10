---
title: RabbitMQ - 消息幂等
categories:
- 中间件
date: 2018-03-24 
tags:
- RabbitMQ
---
#### 什么是消息幂等
publisher重复发送消息，导致业务重复执行。
consumer重复收到消息，导致业务重复执行。

#### 如何处理

`发送消息幂等`
如果在publisher发送消息后，没有接收到ack，publisher会重发消息。
对每条消息，MQ系统内部必须生成一个inner-msg-id，作为去重和幂等的依据。

`消费消息幂等`
如果在consumer消费完消息后，ack没有成功的被broker接收，broker会重发消息。
对每条消息，业务消息体中，必须有一个biz-id，作为去重和幂等的依据
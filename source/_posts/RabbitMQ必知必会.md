---
title: RabbitMQ必知必会
categories:
- 中间件
date: 2018-09-16 
tags:
- RabbitMQ
---
>支持AMQP协议
>用Erlang语言编写，并发性能好
>支持消息持久化
>支持集群扩展
>Spring AMQP支持
>Spring Cloud Stream支持

#### 应用场景

`应用解耦`
消费者可以随意增加，而不需要修改生产者的代码。对于非核心流程，可以放到消息队列中让消息消费者去按需消费，而不影响核心主流程。

`异步处理`
任务异步化，提高系统响应性能。

`限流削峰`
利用消息队列做一个通用的“漏斗”，防止短时间内高流量压垮应用。

#### 组件
`server（broker）`
接收和分发消息的应用，RabbitMQ Server就是消息代理服务器。

`virtualHost`
虚拟的broker，拥有自己的队列、交换器和绑定，拥有自己的权限机制，virtualHost之间是绝对隔离的。

`exchange`
接收消息，消息到达代broker的第一站。
路由消息，根据路由键分发消息到queue。

`Queue`
它是消息的容器，消息最终到达队列中，等待消费者消费。
一个消息可投入一个或多个队列。

`message`
它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。

#### Exchange模式

`Direct Exchange`
这种模式下不需要将Exchange进行任何绑定(binding)操作。
该模式的自带Exchange，称其为default Exchange，名字为空字符串。
消息传递时需要“RouteKey”，为要发送到的队列名字。
如果vhost中不存在RouteKey中指定的队列名，则该消息会被抛弃。

`Fanout Exchange`
不需要RouteKey，转发到与该Exchange绑定(Binding)的所有Queue上。

`Topic Exchange`
这种模式支持路由键的模糊匹配，匹配成功会被路由到相应队列。
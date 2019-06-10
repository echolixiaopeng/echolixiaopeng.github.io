---
title: RabbitMQ - 延迟消息
categories:
- 中间件
date: 2018-01-25 
tags:
- RabbitMQ
---
#### 什么是延迟消息
所谓”延时消息”是指当消息被发送以后，并不想让消费者立即拿到消息，而是等待指定时间后，消费者才拿到这个消息进行消费。
常用来处理“超时取消”，“定时执行”等业务场景。
#### 如何实现

`实现原理`
message达到生存时间，变为dead letter，被转发到指定的队列。
对Queue设置x-expires，或对Message设置x-message-ttl，来控制消息的生存时间。
配置x-dead-letter-exchange和x-dead-letter-routing-key，重新路由转发到指定的队列。

`配置示例`

```
@Component
public class DelayQueue {

    @Autowired
    private RabbitTemplate rabbitTemplate;

    @Override
    @RabbitListener(queues = DEAD_QUEUE)
    public void doJob(Message token) {
        
        //发送延迟消息
        rabbitTemplate.convertAndSend(BLOCKED_QUEUE, new Date(), message -> {
            message.getMessageProperties().setExpiration("120000");
            return message;
        });

    }

    //定义延迟消息队列
    @Bean
    public Queue blockedQueue() {
        return QueueBuilder
            .durable(BLOCKED_QUEUE)
            //绑定dead letter到Dead Letter Exchanges（DLX）
            .withArgument("x-dead-letter-exchange", "")
            .withArgument("x-dead-letter-routing-key", TOKEN_QUEUE)
            .build();
    }

    //绑定延迟消息队列，到default exchange
    @Bean
    public Binding bindBlockedQueue() {
        return BindingBuilder
            .bind(blockedQueue())
            .to(DirectExchange.DEFAULT)
            .with(BLOCKED_TOKEN_QUEUE);
    }
    
    //定义dead letter接收队列
    @Bean
    public Queue deadQueue() {
        return QueueBuilder
            .durable(TOKEN_QUEUE)
            .build();
    }

    //绑定dead letter接收队列，到default exchange
    @Bean
    public Binding bindDeadQueue() {
        return BindingBuilder
            .bind(deadQueue())
            .to(DirectExchange.DEFAULT)
            .with(TOKEN_QUEUE);
    }
}

```
#### 注意
如果在同一queue中，message分配不同的ttl值，message可能会被延迟dead。
在延迟队列中，所有的消息等待队列头部消息过期后，开始延迟倒计时。
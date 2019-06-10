---
title: 杂谈Spring Framework
categories:
- 开源框架
date: 2018-07-12 
tags:
- Spring
---
* `Bean的生命周期`
>实例化bean对象
>注入对象的属性
>调用BeanNameAware.setBeanName()
>调用BeanFactoryAware.setBeanFactory()
>调用ApplicationContextAware.setApplicationContext()
>调用BeanPostProcessor.postProcessBeforeInitialzation()
>调用InitializingBean.afterPropertiesSet()
>执行init-method
>调用BeanPostProcessor.postProcessAfterInitialization()
>使用该Bean
>调用DisposableBean.destroy()
>执行destroy-method方法

* `Bean的作用域`
>singleton，容器在启动时,自动实例化一个实例。
>prototype，对bean请求的时，创建一个新的的实例。
>request，对一次的http的请求，创建一个全新的bean实例。
>session，对某个http seeision 同生通灭。
>globalSession，对应全局Session的生命周期范围内。


* `Spring MVC的工作原理`
>客户端请求统一到达DispatcherServlet。
>根据HandlerMapping获取对应的Handler。
>Handler完成请求后，返回ModelAndView对象给DispatcherServlet。
>根据ViewResolver完成视图解析
>DispatcherServlet会利用视图对象对模型数据进行渲染。

* `BeanFactory 与 ApplicationContexts`
>ApplicationContext继承BeanFactory
>ApplicationContexts提供了更高级的功能。
>BeanFactory在启动时不加载实例，后者反之。

* `事务管理`
>事务四特性(ACID)：原子性，一致性，隔离性，持久性
>编程式事务管理：灵活性，但是难维护。
>声明式事务管理：业务代码和事务管理分离。

* `隔离级别`
>READ_UNCOMMITTED：允许读取尚未提交的更改。
>READ_COMMITTED：允许从已经提交的并发事务读取。
>REPEATABLE_READ：对相同字段的多次读取的结果是一致的。
>SERIALIZABLE：确保不发生脏读、不可重复读和幻影读。

* `传播行为`
>REQUIRED 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
REQUIRES_NEW 创建一个新的事务，如果当前存在事务，则把当前事务挂起。
SUPPORTS 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
NOT_SUPPORTED 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
NEVER 以非事务方式运行，如果当前存在事务，则抛出异常。
MANDATORY 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
NESTED 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。

* `工具类中注入bean`
>加上@component和@PostConstruct注解。
>@PostConstruct服务器启动时的做一些初始化工作。

* `AOP的代理实现`
>动态代理，基于JDK动态代理或CGLIB动态代理，jdk动态代理必须是目标类基于接口。
静态代理，编译时进行织入，AspectJ

* `BeanPostProcessor`
> 在Spring容器中完成bean实例化、配置以及其他初始化方法前后要添加一些自己逻辑处理。
> @Value的实现逻辑依赖于此。

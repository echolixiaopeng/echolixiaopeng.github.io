---
title: 浅识Nginx
categories:
- 中间件
date: 2018-06-01 
tags:
- Nginx
---
>国籍：俄罗斯
>语言：C语言
>开源软件，BSD许可
>跨平台支持Linux和Windows
>可作为WEB服务器，负载均衡器，邮件代理服务器等

#### 优势
`轻量级`
占用更少的内存资源

`高并发`
异步非阻塞，epoll模，单机高并发

`模块化`
高度模块化的设计，便于扩展

`超稳定`
分为主进程，工作进程

#### 为什么高性能
`多进程模型`
nginx默认以多进程的方式工作，一个master进程和多个worker进程，master进程主要用来管理worker进程。
`异步非阻塞`
在epoll支持下，采用异步非阻塞的方式来处理请求，从而实现高并发。

#### 进程模型
>多进程模型
>独立的工作进程，不共享资源，不加锁

`master进程`
主进程，充当监控进程。
不需要处理网络事件，不负责业务的执行。
只负责管理worker进程。

`worker进程`
负责处理网络IO。
各worker进程竞争来自客户端的请求。
多cpu的情况下，设置woker进程的数量等于cpu的核数。
Nginx支持将某一个进程绑定在某一个核上。

#### 常用命令
`nginx` 启动

`nginx -s stop` 关闭

`ngixn -s relaod` 重载配置文件

`nginx -t` 检查配置文件

`nginx -v` 查看版本

#### 性能优化

1. worker_processes配置为CPU核数。
2. 将worker process与指定cpu核绑定。
3. 关闭访问日志，加快磁盘IO。
4. 开启gzip，减少网络传输时间。
5. worker_rlimit_nofile，更改worker进程的最大打开文件数限制
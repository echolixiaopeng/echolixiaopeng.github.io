---
title: Nginx应用 - 虚拟主机
categories:
- 中间件
date: 2017-10-13 
tags:
- Nginx
---
#### 应用场景
基于域名的虚拟主机。
同一IP，不同域名，解析到不同的服务。

#### 配置示例
```
server {
    listen       80;
    server_name  www.a.com;

    location / {
        //转发到7421端口
        proxy_pass   http://127.0.0.1:7421;
    }
}

server {
    listen       80;
    server_name  www.b.com;

    location / {
        //转发到7422端口
        proxy_pass   http://127.0.0.1:7422;
    }
}
```
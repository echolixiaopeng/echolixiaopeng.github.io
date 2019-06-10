---
title: Nginx应用 - 负载均衡
categories:
- 中间件
date: 2018-06-01 
tags:
- Nginx
---
#### 负载策略
`轮循（默认）`
根据Nginx配置文件中的顺序，依次把客户端的Web请求分发到不同的后端服务器上。

`权重`
配置Nginx把请求更多地分发到高配置的后端服务器上，把相对较少的请求分发到低配服务器。

`IP Hash`
同一客户端连续的Web请求都会被分发到同一服务器进行处理。

`最少连接`
请求会被转发到连接数最少的服务器上。

#### 配置示例
* 轮循（默认）

```
upstream service {
    server 192.168.1.7;
    server 192.168.1.8;
}

server {
    listen       80;
    location / {
        proxy_pass_header Server;
        #保留Host请求头
        proxy_set_header Host $http_host;
        #保留用户真实IP
        proxy_set_header X-Real-IP $remote_addr;
        #对应upstream的配置
        proxy_pass   http://service;
    }
}
```

* 权重
```
upstream service {
    server 192.168.1.10 weight=1;
    server 192.168.1.11 weight=2;
}
```
* IP Hash
```
upstream service {
    ip_hash;
    server 192.168.1.7;
    server 192.168.1.8;
}
```

* 最少连接
```
upstream service {
    least_conn;
    server 192.168.1.7;
    server 192.168.1.8;
}
```
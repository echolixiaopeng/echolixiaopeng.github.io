---
title: Nginx应用 - 反向代理
categories:
- 中间件
date: 2018-06-01 
tags:
- Nginx
---
#### 为什么要用？
1. 保护源服务的安全，相当于堡垒服务。
2. 缓存静态资源，加速web请求。
3. 灵活的服务中间件，实现服务热部署。

#### 配置示例
```
server {
    listen       7421;

    location / {
        proxy_pass_header Server;
        //保留Host请求头
        proxy_set_header Host $http_host;
        //保留用户真实IP
        proxy_set_header X-Real-IP $remote_addr;
        //转发到7422端口
        proxy_pass   http://127.0.0.1:7422;
    }
}
```




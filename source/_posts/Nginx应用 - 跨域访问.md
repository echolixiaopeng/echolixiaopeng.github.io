---
title: Nginx应用 - 跨域访问
categories:
- 中间件
date: 2018-10-13 
tags:
- Nginx
---
#### 跨域问题
跨域，指的是浏览器不能执行其他网站的脚本。 
它是由浏览器的同源策略造成的，是浏览器施加的安全限制。
所谓同源是指，域名，协议，端口均相同。
Nginx代替后端服务，处理跨域问题。

#### 配置示例
```
location / {  
    if ($request_method = 'OPTIONS') {
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    return 204;
    }
} 
```
---
title: Nginx应用 - Nginx应用 - Https
categories:
- 中间件
date: 2017-10-08 
tags:
- Nginx
---
#### 应用场景
Nginx代替后端服务，处理Https问题。

#### 配置示例
```
server {
    listen 443;
    server_name localhost;
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/215069382450020.pem;
    ssl_certificate_key  cert/215069382450020.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root html;
        index index.html index.htm;
    }
}
```
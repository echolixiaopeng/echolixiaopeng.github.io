---
title: Nginx应用 - 流量镜像
categories:
- 中间件
date: 2018-10-13 11:11:11
tags:
- Nginx
---
#### 应用场景
版本发布前进行预先验证；
使用生产流量进行压测；
复制客户端网络请求为两份，原始请求的返回将作为客户端相应，镜像请求将被忽略；
通过ngx_http_mirror_module 模块，实现的该功能；

#### 配置示例
```
location / {
    mirror /mirror;
    #off，会丢失body
    mirror_request_body on;
    proxy_pass http://127.0.0.1:9502;
}
 
location /mirror {
    internal;
    proxy_pass http://127.0.0.1:8081$request_uri;
    proxy_set_header X-Original-URI $request_uri;
}

```
---
title: Nginx应用 - URL拦截
categories:
- 中间件
date: 2018-06-01 
tags:
- Nginx
---
#### 应用场景
临时下线某个业务接口，不重新发布服务。


#### 配置示例
```
location / {

#匹配请求url中包含指定字符的请求
if ($request_uri ~* "/bad_request") {
    return 200 "error";
}
```
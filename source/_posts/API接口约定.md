---
title: API接口约定
categories:
- 方法论
date: 2018-08-19
tags:
- RESTful
---
> 站在巨人的肩膀上 ——牛顿

## 全局约定

    1. 仅支持发送和接收UTF-8编码数据
    2. request和response，统一为application/json
    3. response返回结构体{code:0,msg:'ok',data:{key:value}}
    4. token统一采用JWT标准
    5. token统一为header参数
    5. 日期统一使用10位UNIX时间戳
    6. 对外的接口统一采用HTTPS协议
    7. 在路径中设置版本，如 example.com/v1
    8. response的httpcode统一为200
    9. response中的code，0表示成功，400-499为客户端全局错误，500-599为服务端全局错误，其他为接口局部错误
 
## URI表达
> 实例均摘录于《GitHub API v3 接口文档》

1. 获取单个资源

        [GET] 
        /repos/:owner/:repo/issues/:number
        /repos/octocat/Hello-World/issues/1347


2. 获取多个资源

        [GET]
        /repos/:owner/:repo/issues
        /repos/octocat/Hello-World/issues
        
3. 分页查询

        [GET]
        /repos/:owner/:repo/issues?page=:pageNum&per_page=pageSize
        /repos/octocat/Hello-World/issues?page=3&per_page=100
        
        
4. 分页搜索

        [GET]
        /repos/:owner/:repo/issues?state=:state&sort=:sort&direction=:direction
        /repos/octocat/Hello-World/issues?state=closed&sort=created&direction=desc

5. 添加资源

        [POST]
        /repos/:owner/:repo/issues
        /repos/octocat/Hello-World/issues

6. 删除资源

        [DELETE]
        /repos/:owner/:repo
        /repos/octocat/Hello-World
        
7. 修改资源

        [PATCH]
        /repos/:owner/:repo
        /repos/octocat/Hello-World
        
8. 动作型请求(把动作转换成资源)
    
        [POST]
        /repos/:owner/:repo/forks
        /repos/octocat/Hello-World

        



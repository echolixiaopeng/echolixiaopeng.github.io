---
title: 排错卷宗 - redis连接timeout
categories:
- 排错卷宗
date: 2018-09-25 
tags:
- redis
---
#### 场景还原
>`小A`：奇怪！生产环境service-a这两天怎么偶尔会报redis的连接timeout错误？
>`小A`：这个错误出现的频率毫无规律，而且每次只有一两个报错。
>`小A`：难道是本机到redis服务器的网络有抖动？
>`小A`：不回呀。内网环境下不应该有这样的抖动。
>`小A`：咦？其他service会不会也有这样的报错？
>`小A`：要是其他service也有，岂不是redis端的问题？
>`小A`：小B，你负责的service-b这几天有没有redis的连接timeout报错？
>`小B`：小A哥，service-b这几天真有redis的连接timeout报错！你料事如神呀！
>`小A`：小C，小D，小E，你们都看下，是不是有相同情况？
>`小C`：有！
>`小D`：也有！
>`小E`：+1
>`小A`：真相永远只有一个！一定是redis端出现了问题！

#### 问题定位
`慢查询阻塞`
redis是单线程处理的。一些耗时长的命令会造成阻塞，如keys、sort等命令。应该在生产环境禁用这些危险的命令。

`SLOWLOG命令`
SLOWLOG GET获取所有慢日志；SLOWLOG GET N获取最近N条慢日志；SLOWLOG LEN获取当前慢日志长度；

`慢查询日志配置`
slowlog-max-len线上可以设置为1000以上；slowlog-log-lower-than建议设置为1毫秒；
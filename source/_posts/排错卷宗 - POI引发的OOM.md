---
title: 排错卷宗 - POI引发的OOM
categories:
- 排错卷宗
date: 2017-10-21 
tags:
- OOM
---
#### 场景还原
>`小A`：该死！service-a又假死了，马上重启一波。
>`小A`：肯定又有运营导出表格数据了。几万条地导，服务就撑不住了。
>`小A`：这次一定要在技术角度解决这个问题！
>`小A`：简单来说，就是POI类库在导出Excel是引发了OOM问题。
>`小A`：业务导出的数据量大，JVM又不能成功的回收这些POI的对象，该怎么办呐？

#### 问题定位
`jmap堆栈分析`
使用jmap工具列出堆内存中所有对象的名称和数目。果然是POI的锅！SXSSFRow，SXSSFCell这几个POI的对象大量占用了堆内存。

`SXSSFWorkbook`
SXSSFWorkbook提供了OOM的解决方案，会临时写到临时文件中，避免OOM。


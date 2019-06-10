---
title: 浅析JVM调试工具
categories:
- JAVA
date: 2018-01-09 
tags:
- JVM
---
>JVM调试工具，适用于调试以下问题：
>OOM问题
>内存泄露问题
>CPU占用率高问题

#### top - 系统进程监控
```
top -Hp <pid>

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                                  
16786 root      20   0 5207600 851948   9692 S  0.7  2.6   1:31.38 java                                                     
16783 root      20   0 5207600 851948   9692 S  0.3  2.6   2:09.71 java                                                     
16785 root      20   0 5207600 851948   9692 S  0.3  2.6   1:31.53 java                                                    
```
`参数说明`
>-H 检查该进程内运行的线程状况
>-p 通过指定监控进程ID来仅仅监控某个进程的状态。 


`结果列说明`
>PID : 进程id
>S : 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
>%CPU : 上次更新到现在的CPU时间占用百分比
>%MEM : 进程使用的物理内存百分比
>TIME+ : 进程使用的CPU时间总计（1:31.38，表示1分钟:31秒.38毫秒）


#### jps - java虚拟机进程监控
```
jps -ml 

23424 /data/service-x-test/gateway-x/gateway-x.jar
28098 service-xx-x-0.0.1-SNAPSHOT.jar 
5256 gateway-xx.jar --spring.profiles.active=test 
```
`参数说明`
>-m:输出主函数传入的参数
>-l: 输出应用程序主类完整package名称或jar完整名称

#### jstatck - 查看Java进程内的线程堆栈信息 
```
jstack <pid>

"Druid-ConnectionPool-Create-641011362" #26 daemon prio=5 os_prio=0 tid=0x00007f55898de000 nid=0x4165 waiting on condition [0x00007f55c3afd000]
   java.lang.Thread.State: WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x00000000b6a23ca0> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(LockSupport.java:175)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2039)
        at com.alibaba.druid.pool.DruidDataSource$CreateConnectionThread.run(DruidDataSource.java:2308)
```
`结果列说明`
>nid : 操作系统映射的线程id。16进制表示。
>Thread.State : Waiting(等待唤醒)，Timed_Waiting(有时间限制的等待唤醒)，Runnable(得到CPU，就可以执行)


#### jmap
* 查看指定JVM进程的堆信息
```
jmap -heap <pid>

Attaching to process ID 16694, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.144-b01

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration: #堆内存初始化配置
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   #-XX:MaxHeapSize=设置JVM堆的最大大小
   MaxHeapSize              = 1258291200 (1200.0MB)
   #-XX:NewSize=设置JVM堆的‘新生代’的默认大小
   NewSize                  = 175112192 (167.0MB)
   #-XX:MaxNewSize=设置JVM堆的‘新生代’的最大大小
   MaxNewSize               = 419430400 (400.0MB)
   #-XX:OldSize=设置JVM堆的‘老生代’的大小
   OldSize                  = 351272960 (335.0MB)
   #-XX:NewRatio=:‘新生代’和‘老生代’的大小比率
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space: #Eden区内存分布
   capacity = 62914560 (60.0MB)
   used     = 5889832 (5.616981506347656MB)
   free     = 57024728 (54.383018493652344MB)
   9.36163584391276% used
From Space: #其中一个Survivor区的内存分布
   capacity = 1048576 (1.0MB)
   used     = 589824 (0.5625MB)
   free     = 458752 (0.4375MB)
   56.25% used
To Space: #另一个Survivor区的内存分布
   capacity = 1572864 (1.5MB)
   used     = 0 (0.0MB)
   free     = 1572864 (1.5MB)
   0.0% used
PS Old Generation #当前的Old区内存分布 
   capacity = 376963072 (359.5MB)
   used     = 114052600 (108.76903533935547MB)
   free     = 262910472 (250.73096466064453MB)
   30.25564265350639% used

40297 interned Strings occupying 4669600 bytes.
```
`注意`
jmap -heap有些时候会导致进程暂停，可通过jstat -gc获取内存目前每个区域的使用状况

 * 打印堆的对象统计
```
jmap -histo <pid>

 num     #instances         #bytes  class name
----------------------------------------------
   1:         41799       30989904  [I
   2:        299256       26082000  [C
   3:         72156        8964856  [B
   4:        291594        6998256  java.lang.String
   5:         52692        4636896  java.lang.reflect.Method
```
`结果列说明`
>B 代表 byte
C 代表 char
D 代表 double
F 代表 float
I 代表 int
J 代表 long
Z 代表 boolean
前边有 [ 代表数组， [I 就相当于 int[]
对象用 [L+ 类名表示

* 导出JVM内存信息
```
jmap -dump:format=b,file=文件名 <pid>
```
`结果说明`
>导出的dump文件可以使用Java VisualVM 分析。
>heap如果比较大的话，就会导致这个过程比较耗时。
>执行的过程中为了保证dump的信息是可靠的，所以会暂停应用。
>配置JVM启动参数，OOM时自动生成Dump文件。

#### jstat
```
jstat -gc <pid> <采集间隔时间ms> <采集次数>

 S0C    S1C    S0U    S1U      EC       EU        OC         OU       MC     MU    CCSC   CCSU   YGC     YGCT    FGC    FGCT     GCT   
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
512.0  512.0   96.0   0.0   56320.0  56320.0   273408.0   38963.4   48384.0 46119.9 6144.0 5724.3   6712   22.214   2      0.216   22.429
```
`结果列说明`
>YGC : 从应用程序启动到采样时年轻代中gc次数
>YGCT : 从应用程序启动到采样时年轻代中gc所用时间(s)
>FGC : 从应用程序启动到采样时old代(全gc)gc次数
>FGCT : 从应用程序启动到采样时old代(全gc)gc所用时间(s)
>OC :  Old代的容量 (字节)
>OU :  Old代目前已使用空间 (字节)



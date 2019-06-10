---
title: 浅析JAVA集合
categories:
- JAVA
date: 2018-06-08 
tags:
- JVM
---
>Set，无序、不可重复的集合。
>List，有序、可重复的集合。
>Queue，队列集合实现。
>Map，具有映射关系的集合。
>数组需要在初始化的时候指定长度，集合可以保存不确定数量的数据。
>集合只能保存对象，数组可以保存基本数据类型和对象。

#### Collection 接口
`Collection`是`Set`，`List`，`Queue`的父接口。
`add(E e)`，添加元素。
`size()`方法，返回元素数。
`toArray(T[] a)`方法，返回所有元素的数组。
`remove(Object o)`方法，移出单个元素。
`removeAll(Collection<?> c)`，移出指定元素。
`retainAll(Collection<?> c)`，取交集。
`isEmpty()`方法，size=0返回true。
`iterator()`方法，该方法的返回值是`Iterator\<E\>`。
Iterator 适合访问`链式结构`，for循环适合访问`顺序结构`。
在LinkedList里，使用iterator较快；在ArrayList里，for循环较快。
foreach和iterator遍历集合时不能修改元素。
使用Collections的sync方法包装集合为线程安全的集合类。

##### Set 接口
>Set集合不允许包含相同的元素,集合是无序的。
>Set的实现原理是基于Map的。

`HashSet`
线程不安全；
实现：建立一个Map，“键”就是我们要存入的对象，“值”则是一个常量；
允许包含值为null的元素，但最多只能有一个null元素；

`LinkedHashSet`
线程不安全；
也是一个hash表，但是同时维护了一个双链表来记录插入的顺序；
遍历该集合时候，LinkedHashSet将会以元素的添加顺序访问集合的元素；
插入性能略低于HashSet，但是遍历性能优于HashSet；

`TreeSet`
线程不安全；
基于Map来实现；
其底层结构为红黑树，性能不如HashSet；
遍历该集合时候，会以排序顺序方位集合元素。
素需要实现Comparable接口，或者集合中传入自定义比较器；

##### List 接口
>List集合允许包含相同的元素，集合是有序的。
>每个元素都有其对应的顺序索引。

`ArrayList`
线程不安全；
基于动态数组实现；
添加元素时会扩容数组，删除元素时不会缩小数组，trimToSize方法可以手动缩小；
内部数组默认大小为10，减少动态分配次数，可提高性能；

`LinkedList`
线程不安全；
基于双向链表实现；
插入和删除性能优于ArrayList，查询性能不如ArrayList；

`Vector`
线程安全；

##### Queue 接口
>FIFO容器，队列头部的元素保存时间最长，尾部反之；
>Deque是Queue的子接口，为双端队列，LinkedList为Deque的实现；
>add()方法，添加元素到尾部；
>element()方法，获取头部元素，但不删除；
>peek()方法，获取头部元素，但不删除；
>poll()方法，获取头部元素，并删除；
>remove()方法，删除头部元素；

`PriorityQueue`
队列的顺序按照大小排序，不按照插入顺序。

#### Map 接口
>Key和Value必须是引用类型；
>Key不允许重复；

`HashMap`
线程不安全；
Key和Value都可以是null；
实现基于数组和链表，key值hash取模值分布在数组上，entry形成链表；
扩容时，生成新的数组，并重新计算写入。
效率优于HashTable；

`HashTable`
线程安全；
Key和Value都不可以是null；

`LinkedHashMap`
线程不安全；
实现基于HashMap+LinkedList，LinkedList维护插入元素的先后顺序；
迭代顺序就是是插入顺序；
性能低于HashMap，需要维护顺序链表；

`TreeMap`
红黑树结构；
根据Key的大小进行排序；

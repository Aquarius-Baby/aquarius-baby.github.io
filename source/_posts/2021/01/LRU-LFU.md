---
title: LRU/LFU
categories:
  - Java
tags:
  - 标签1
date: 2021-01-26 20:22:17
---
# LRU
Least Recently Used，最近最久未使用算法

是一种常见的缓存算法，在很多分布式缓存系统（如Redis, Memcached）中都有广泛使用。

LRU算法的思想是：如果一个数据在最近一段时间没有被访问到，那么可以认为在将来它被访问的可能性也很小。因此，当空间满时，最久没有访问的数据最先被置换（淘汰）。

LRU算法的描述： 设计一种缓存结构，该结构在构造时确定大小，假设大小为 K，并有两个功能：

set(key,value)：将记录(key,value)插入该结构。当缓存满时，将最久未使用的数据置换掉。
get(key)：返回key对应的value值。
实现：最朴素的思想就是用数组+时间戳的方式，不过这样做效率较低。因此，我们可以用双向链表（LinkedList）+哈希表（HashMap）实现（链表用来表示位置，哈希表用来存储和查找），在Java里有对应的数据结构LinkedHashMap。



# LFU
Least Frequently Used，最近最少使用算法
找到队列中使用频率最少
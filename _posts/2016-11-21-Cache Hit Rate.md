---
title: 缓存命中率
header-img: img/fantasy.jpg
tags: 计算机科学
published: false
---

### 缓存的意义

+ 为什么要有缓存？

CPU 的缓存（L1 L2 L3）是 CPU 和内存之间的临时存储器，它的速度非常快，可以极大缓解内存速度慢导致 CPU 等待数据传输的问题。

其实可以把这个概念扩展一下，如果有一些数据需要从硬盘上读取，那么可以用内存当做这些资源的“缓存”。通过设计一个合理的缓存机制，可以减少程序从硬盘读取数据的频率，增加程序运行效率。

例如，游戏内需要使用大量不同的纹理，那么可以设计一个 LRU 队列来缓存部分纹理，这样就不用每次都从磁盘加载了，这样可以提高性能。

+ 缓存应该多大？

缓存速度固然很快，但是大部分机器的内存都是非常有限的，尤其是各种移动设备。所以缓存的尺寸应该尽量小，当然如果内存很大，那把所有东西都缓存起来也是可以的！不过绝大部分情况下都不会这么干，因为内存非常宝贵。所以如何利用有限的内存来当做缓存就需要用到巧妙设计的数据结构了。

### LRU（Least Recently Used）


参考资料：

+ [CPU 缓存](http://baike.baidu.com/link?url=y4C2scvEykfPoMVwIMOiTUyp1usroKviePQIWhLzGUIJ79OlZ33bywVo_fIP9EjG76GQ6Fg_OYRtwie0vFkzHtPOUdPrazp3gbKynb6NnxC)
+ [有效提高命中率的缓存设计](http://www.cnblogs.com/smark/archive/2013/01/23/2874012.html)
+ [Wikipedia LRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)

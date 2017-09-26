
## 引言

已经步入大四，下面很长的一段时间，将会对面试题进行总结，
java开发面试，重点关注内容为：java基础（重点是IO、集合框架、多线程、反射），数据库（主要是MySQL）、javaweb（Servlet、jsp等）、框架（struts、Hibernate、Spring、SpringMVC、Mybatis等）、java设计模式（最会被提问到的模式）、java8、NIO、JUC、Linux、数据结构（主要面向java，包括链表、栈、队列、二叉树、图等）、前端基础（HTML、CSS、JS、Jquery等）、算法（排序、查找、全排列、动态规划、贪心、分治算法等）、Redis、Maven、Git、高并发下的数据处理。

另外，如果还有余力的话，会对一下内容进行总结：SOA、Dubbo、JVM调优、数据库调优、Spring Boot、Nginx。

SDN开发面试，重点关注内容为：SDN基础，流表的下发、传统网络知识（ARP、TCP、IP，net,DDOS等）、Vxlan、Openflow协议分析、opendaylight重点模块分析（OpenflowPlugin、network-topology等模块）、ODL开发遇到的问题、Yang文件基础总结，RPC、Ovsdb协议、OSGI框架。

另外，如果还有余力的话，会对以下内容进行总结：SD-WAN，ODL源码分析、Openstack等


本文仅仅针对面试可能出现的问题，故所讲的内容可能不会比较全面，同时也会和之前的文章的内容有些重复。

本文会随着面试的深入，**会不定期更新** 

本文仅仅起到一个目录的作用，同时本文的很多内容来源于互联网，在这里做一个声明。



## java篇

### java基础

+ [说说Java接口和抽象类的区别](https://github.com/zlnnjit/interview/blob/master/java/说说Java接口和抽象类的区别.md)

+ [equals()和hashCode()总结](https://github.com/zlnnjit/interview/blob/master/java/equals()%E5%92%8ChashCode()%E6%80%BB%E7%BB%93.md)

+ [ String str="ubuntu"以及String str = new String("ubuntu")的内存分配问题](https://github.com/zlnnjit/interview/blob/master/java/%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E9%97%AE%E9%A2%98.md)

+ [java代码的执行顺序](http://www.bcoder.top/2017/05/25/java%E4%BB%A3%E7%A0%81%E7%9A%84%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F/)


+ [Python元组](https://github.com/zlnnjit/interview/blob/master/python/Python%E5%85%83%E7%BB%84.md)

+ [JAVA中Stack和Heap的区别](http://www.bcoder.top/2017/05/25/JAVA%E4%B8%ADStack%E5%92%8CHeap%E7%9A%84%E5%8C%BA%E5%88%AB/)



+ [java Object类常见的问题](http://www.bcoder.top/2017/05/22/java-Object%E7%B1%BB/)



+ [解释Java对象序列化](http://www.bcoder.top/2017/05/13/%E7%90%86%E8%A7%A3Java%E5%AF%B9%E8%B1%A1%E5%BA%8F%E5%88%97%E5%8C%96/)

+ [java异常总结](http://www.bcoder.top/2017/06/03/java-%E5%BC%82%E5%B8%B8%E6%9C%BA%E5%88%B6/)

+ [什么是内存泄漏？怎样解决？](http://www.bcoder.top/2017/03/22/JAVA%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%88%86%E6%9E%90/)


### java集合




+ [ArrayList、LinkedList、Vector的区别](https://github.com/zlnnjit/interview/blob/master/java/%C2%96ArrayList%E3%80%81LinkedList%E3%80%81Vector%E7%9A%84%E5%8C%BA%E5%88%AB.md)




+ [ArrayList如何实现扩容](https://github.com/zlnnjit/interview/blob/master/java/ArrayList%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%89%A9%E5%AE%B9.md)


+ [Collections常见问题总结](http://www.bcoder.top/2017/03/15/Collections%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93/)


+ [HashMap的数据结构是什么？如何实现的](https://github.com/zlnnjit/interview/blob/master/java/HashMap%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E7%9A%84.md)

+ [HashMap、HashTable的区别](https://github.com/zlnnjit/interview/blob/master/java/HashMap%E3%80%81HashTable%E7%9A%84%E5%8C%BA%E5%88%AB.md)

### java多线程

+ [java有哪几种线程状态？如何转换？](https://github.com/zlnnjit/interview/blob/master/java/java%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%EF%BC%9F%E5%A6%82%E4%BD%95%E8%BD%AC%E6%8D%A2%EF%BC%9F.md)

+ [JAVA线程中BLOCKED和WAITING有什么区别?](https://github.com/zlnnjit/interview/blob/master/java/JAVA%E7%BA%BF%E7%A8%8B%E4%B8%ADBLOCKED%E5%92%8CWAITING%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB.md)

+ [进程和线程的区别,进程间如何通信？](https://github.com/zlnnjit/interview/blob/master/java/%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%8C%BA%E5%88%AB%2C%E8%BF%9B%E7%A8%8B%E9%97%B4%E5%A6%82%E4%BD%95%E9%80%9A%E4%BF%A1%EF%BC%9F.md)



+ [java线程如何通信](https://github.com/zlnnjit/interview/blob/master/java/java%E7%BA%BF%E7%A8%8B%E5%A6%82%E4%BD%95%E9%80%9A%E4%BF%A1.md)


+ [使用java简单写一个死锁，并说明如何避免死锁](https://github.com/zlnnjit/interview/blob/master/java/%E4%BD%BF%E7%94%A8java%E7%AE%80%E5%8D%95%E5%86%99%E4%B8%80%E4%B8%AA%E6%AD%BB%E9%94%81%EF%BC%8C%E5%B9%B6%E8%AF%B4%E6%98%8E%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8D%E6%AD%BB%E9%94%81.md)

### JVM

+ [JVM如何加载字节码文件?](https://github.com/zlnnjit/interview/blob/master/java/JVM%E5%A6%82%E4%BD%95%E5%8A%A0%E8%BD%BD%E5%AD%97%E8%8A%82%E7%A0%81%E6%96%87%E4%BB%B6.md)

+ [简述JVM垃圾回收机制（GC）](https://github.com/zlnnjit/interview/blob/master/java/简述JVM垃圾回收机制（GC）.md)

<br><br><br><br>
## SDN篇
+ [Openflow1.3协议分析](http://www.bcoder.top/2017/09/01/OpenFlow1-3%E6%80%BB%E7%BB%93/)


<br><br><br><br>

## 网络篇
+ [TCP为什么需要三次握手和四次挥手？](https://github.com/zlnnjit/interview/blob/master/java/TCP%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B.md)

<br><br><br><br>

## 数据库篇
### SQL

+ [MySQL基础部分要点](https://github.com/zlnnjit/interview/blob/master/java/MySQL%E5%9F%BA%E7%A1%80%E9%83%A8%E5%88%86%E8%A6%81%E7%82%B9.md)

+ [MySQL进阶部分要点](https://github.com/zlnnjit/interview/blob/master/java/MySQL%E8%BF%9B%E9%98%B6%E9%83%A8%E5%88%86%E8%A6%81%E7%82%B9.md)


+ [MySQL高级部分要点](https://github.com/zlnnjit/interview/blob/master/java/MySQL%E9%AB%98%E7%BA%A7%E9%83%A8%E5%88%86%E8%A6%81%E7%82%B9.md)


+ [索引有什么用？如何建索引？](https://github.com/zlnnjit/interview/blob/master/java/%E7%B4%A2%E5%BC%95%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8%EF%BC%9F%E5%A6%82%E4%BD%95%E5%BB%BA%E7%B4%A2%E5%BC%95%EF%BC%9F.md)

+ [MySQL索引有哪几种？](https://github.com/zlnnjit/interview/blob/master/java/MySQL%E7%B4%A2%E5%BC%95%E5%88%86%E4%B8%BA%E5%93%AA%E5%87%A0%E7%A7%8D%EF%BC%9F.md)


+ [MySQL的联合索引什么情况下可以生效？联合索引和单列索引有什么区别？](https://github.com/zlnnjit/interview/blob/master/java/MySQL%E7%9A%84%E8%81%94%E5%90%88%E7%B4%A2%E5%BC%95%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E5%8F%AF%E4%BB%A5%E7%94%9F%E6%95%88%EF%BC%9F%E8%81%94%E5%90%88%E7%B4%A2%E5%BC%95%E5%92%8C%E5%8D%95%E5%88%97%E7%B4%A2%E5%BC%95%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F.md)

+ [简述事务的特性以及事务的隔离级别](https://github.com/zlnnjit/interview/blob/master/java/%E7%AE%80%E8%BF%B0%E4%BA%8B%E5%8A%A1%E7%9A%84%E7%89%B9%E6%80%A7%E4%BB%A5%E5%8F%8A%E4%BA%8B%E5%8A%A1%E7%9A%84%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB.md)

### NoSQL



<br><br><br><br>
## javaweb篇
+ [Cookie和Session的区别？](https://github.com/zlnnjit/interview/blob/master/java/Cookie%E5%92%8CSession%E7%9A%84%E5%8C%BA%E5%88%AB.md)

+ [jdbc总结](http://www.bcoder.top/2017/03/25/jdbc%E5%A4%8D%E4%B9%A0%E6%80%BB%E7%BB%93/)

+ [Servlet处理请求的流程，有哪些方法？](https://github.com/zlnnjit/interview/blob/master/java/servlet%E5%A4%84%E7%90%86%E8%AF%B7%E6%B1%82%E7%9A%84%E6%B5%81%E7%A8%8B%EF%BC%8C%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E6%B3%95%EF%BC%9F.md)
<br><br><br><br>
## 框架篇

### Spring框架


### SpringMVC框架
+ [SpringMVC拦截器有哪些方法？多个拦截器执行时方法调用的顺序？](https://github.com/zlnnjit/interview/blob/master/java/SpringMVC%E6%8B%A6%E6%88%AA%E5%99%A8%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E6%B3%95%EF%BC%9F%E5%A4%9A%E4%B8%AA%E6%8B%A6%E6%88%AA%E5%99%A8%E6%89%A7%E8%A1%8C%E6%97%B6%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8%E7%9A%84%E9%A1%BA%E5%BA%8F%EF%BC%9F.md)

<br><br><br><br>
## 数据结构与算法篇



+ [二叉树](http://www.bcoder.top/2017/08/12/%C2%96%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E4%B9%8B%E4%BA%8C%E5%8F%89%E6%A0%91/)

+ [什么是二叉查找树？如何用java实现?](https://github.com/zlnnjit/interview/blob/master/java/%E4%BB%80%E4%B9%88%E6%98%AF%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91%EF%BC%9F%E5%A6%82%E4%BD%95%E7%94%A8java%E5%AE%9E%E7%8E%B0.md)




+ [八种常见的排序](http://www.bcoder.top/2017/08/06/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E4%B9%8B%E6%8E%92%E5%BA%8F/)


+ [如何实现全排列?](https://github.com/zlnnjit/interview/blob/master/java/如何实现全排列.md)


<br><br><br><br>
## Linux篇
+ [解释一下Linux软链接和硬链接?](https://github.com/zlnnjit/interview/blob/master/java/Linux%E8%BD%AF%E9%93%BE%E6%8E%A5%E5%92%8C%E7%A1%AC%E9%93%BE%E6%8E%A5.md)

+ [面试中遇到的Linux命令（持续更新）](https://github.com/zlnnjit/interview/blob/master/java/%E9%9D%A2%E8%AF%95%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84Linux%E5%91%BD%E4%BB%A4%EF%BC%88%E6%8C%81%E7%BB%AD%E6%9B%B4%E6%96%B0%EF%BC%89.md)
<br><br><br><br>
## 前端篇


<br><br><br><br>
## java设计模式篇

+ [说说代理模式，并用java实现，说说你在什么地方用到过代理模式](https://github.com/zlnnjit/interview/blob/master/java/说说代理模式，并用java实现，说说你在什么地方用到过代理模式.md)

+ [说说java几种常见的单例模式，并分析优缺点](https://github.com/zlnnjit/interview/blob/master/java/%E8%AF%B4%E8%AF%B4java%E5%87%A0%E7%A7%8D%E5%B8%B8%E8%A7%81%E7%9A%84%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F%EF%BC%8C%E5%B9%B6%E5%88%86%E6%9E%90%E4%BC%98%E7%BC%BA%E7%82%B9.md)

<br><br><br><br>
## java高级篇 



### JUC

+ [什么是ConcurrentHashMap,与HashTable有什么区别？](https://github.com/zlnnjit/interview/blob/master/java/%E4%BB%80%E4%B9%88%E6%98%AFConcurrentHashMap%2C%E4%B8%8EHashTable%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB%EF%BC%9F.md)


+ [介绍一下Synchronized的用法](https://github.com/zlnnjit/interview/blob/master/java/%E4%BB%8B%E7%BB%8D%E4%B8%80%E4%B8%8BSynchronized%E7%9A%84%E7%94%A8%E6%B3%95.md)

+ [介绍一下Lock的用法，并说明与Synchronized的区别](https://github.com/zlnnjit/interview/blob/master/java/介绍一下Lock的用法，并说明与Synchronized的区别.md)


### java8

### NIO


<br><br><br><br>

## 高并发下的数据处理与数据库查询
+ [如何对海量的数据(大文件)进行处理?](https://github.com/zlnnjit/interview/blob/master/java/%E5%A6%82%E4%BD%95%E5%AF%B9%E6%B5%B7%E9%87%8F%E7%9A%84%E6%95%B0%E6%8D%AE(%E5%A4%A7%E6%96%87%E4%BB%B6)%E8%BF%9B%E8%A1%8C%E5%A4%84%E7%90%86.md)


<br><br><br><br>
## 算法题

+ [合并两个已排序的数组，要求去重](https://github.com/zlnnjit/interview/blob/master/编程题/合并两个已排序的数组，要求去重.md)

+ [统计数组中各个元素出现的次数](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E7%BB%9F%E8%AE%A1%E6%95%B0%E7%BB%84%E4%B8%AD%E5%90%84%E4%B8%AA%E5%85%83%E7%B4%A0%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0.md)

+ [设计一个有getMin功能的栈](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E8%AE%BE%E8%AE%A1%E4%B8%80%E4%B8%AA%E6%9C%89getMin%E5%8A%9F%E8%83%BD%E7%9A%84%E6%A0%88.md)

+ [设计一个由两个栈组成的队列](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E8%AE%BE%E8%AE%A1%E4%B8%80%E4%B8%AA%E7%94%B1%E4%B8%A4%E4%B8%AA%E6%A0%88%E7%BB%84%E6%88%90%E7%9A%84%E9%98%9F%E5%88%97.md)

+ [二叉树层次遍历问题(不换行打印与换行打印)](https://github.com/zlnnjit/interview/blob/master/编程题/二叉树层次遍历问题(不换行打印与换行打印).md)

+ [二叉树的序列化与反序列化问题](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%BA%8F%E5%88%97%E5%8C%96%E4%B8%8E%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96%E9%97%AE%E9%A2%98.md)

+ [对几乎已经排好序的数组进行排序](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%AF%B9%E5%87%A0%E4%B9%8E%E5%B7%B2%E7%BB%8F%E6%8E%92%E5%A5%BD%E5%BA%8F%E7%9A%84%E6%95%B0%E7%BB%84%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F.md)

+ [判断数组中是否有重复值。必须保证额外空间复杂度为O(1)](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%88%A4%E6%96%AD%E6%95%B0%E7%BB%84%E4%B8%AD%E6%98%AF%E5%90%A6%E6%9C%89%E9%87%8D%E5%A4%8D%E5%80%BC%E3%80%82%E5%BF%85%E9%A1%BB%E4%BF%9D%E8%AF%81%E9%A2%9D%E5%A4%96%E7%A9%BA%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6%E4%B8%BAO(1).md)

+ [把两个有序数组合并成一个数组，第一个数组空间正好可以容纳两个数组的元素](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E6%8A%8A%E4%B8%A4%E4%B8%AA%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E5%90%88%E5%B9%B6%E6%88%90%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84%EF%BC%8C%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%95%B0%E7%BB%84%E7%A9%BA%E9%97%B4%E6%AD%A3%E5%A5%BD%E5%8F%AF%E4%BB%A5%E5%AE%B9%E7%BA%B3%E4%B8%A4%E4%B8%AA%E6%95%B0%E7%BB%84%E7%9A%84%E5%85%83%E7%B4%A0.md)

+ [荷兰国旗问题](https://github.com/zlnnjit/interview/blob/master/编程题/荷兰国旗问题.md)

+ [在行列都排好序的矩阵中找数](https://github.com/zlnnjit/interview/tree/master/%E7%BC%96%E7%A8%8B%E9%A2%98)

+ [求需要排序的最短子数组长度](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E6%B1%82%E9%9C%80%E8%A6%81%E6%8E%92%E5%BA%8F%E7%9A%84%E6%9C%80%E7%9F%AD%E5%AD%90%E6%95%B0%E7%BB%84%E9%95%BF%E5%BA%A6.md)

+ [判断一颗二叉树是否是二叉树的子树](https://github.com/zlnnjit/interview/blob/master/编程题/判断一颗二叉树是否是二叉树的子树.md)

+ [判断字符串是否互为变形词](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%88%A4%E6%96%AD%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%98%AF%E5%90%A6%E4%BA%92%E4%B8%BA%E5%8F%98%E5%BD%A2%E8%AF%8D.md)

+ [判断字符串是否互为旋转词](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%88%A4%E6%96%AD%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%98%AF%E5%90%A6%E4%BA%92%E4%B8%BA%E6%97%8B%E8%BD%AC%E8%AF%8D.md)

+ [单词逆序](https://github.com/zlnnjit/interview/blob/master/编程题/单词逆序.md)

+ [将字符串在指定位置的两侧字符调换位置](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%B0%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%9C%A8%E6%8C%87%E5%AE%9A%E4%BD%8D%E7%BD%AE%E7%9A%84%E4%B8%A4%E4%BE%A7%E5%AD%97%E7%AC%A6%E8%B0%83%E6%8D%A2%E4%BD%8D%E7%BD%AE.md)

+ [将字符串按照字典序最想的方式拼装](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%B0%86%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%8C%89%E7%85%A7%E5%AD%97%E5%85%B8%E5%BA%8F%E6%9C%80%E5%B0%8F%E7%9A%84%E6%96%B9%E5%BC%8F%E6%8B%BC%E8%A3%85.md)

+ [判断是否是整体有效的括号字符串](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E6%98%AF%E6%95%B4%E4%BD%93%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7%E5%AD%97%E7%AC%A6%E4%B8%B2.md)

+ [求最长无重复字符串的长度](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E6%B1%82%E6%9C%80%E9%95%BF%E6%97%A0%E9%87%8D%E5%A4%8D%E5%AD%97%E7%AC%A6%E4%B8%B2%E7%9A%84%E9%95%BF%E5%BA%A6.md)



+ [实现一个逆序栈](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E5%AE%9E%E7%8E%B0%E4%B8%80%E4%B8%AA%E9%80%86%E5%BA%8F%E6%A0%88.md)


+ [使用辅助栈进行排序](https://github.com/zlnnjit/interview/blob/master/%E7%BC%96%E7%A8%8B%E9%A2%98/%E4%BD%BF%E7%94%A8%E8%BE%85%E5%8A%A9%E6%A0%88%E8%BF%9B%E8%A1%8C%E6%8E%92%E5%BA%8F.md)
























<br><br><br><br>


## 工具篇

### Git

### SVN

### maven
<br><br><br><br>


<br><br><br><br>
## 高級篇

### SOA

### Dubbo

### JVM调优

### 数据库调优


### Spring Boot


### Nginx






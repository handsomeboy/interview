
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


+ [为什么重写HashCode还要重写HashCode?怎样进行重写equals和Hashcode方法?](http://www.bcoder.top/2017/02/19/%E6%80%8E%E6%A0%B7%E8%BF%9B%E8%A1%8C%E9%87%8D%E5%86%99equals%E5%92%8CHashcode%E6%96%B9%E6%B3%95/)



+ [java代码的执行顺序](http://www.bcoder.top/2017/05/25/java%E4%BB%A3%E7%A0%81%E7%9A%84%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F/)



+ [JAVA中Stack和Heap的区别](http://www.bcoder.top/2017/05/25/JAVA%E4%B8%ADStack%E5%92%8CHeap%E7%9A%84%E5%8C%BA%E5%88%AB/)



+ [java Object类常见的问题](http://www.bcoder.top/2017/05/22/java-Object%E7%B1%BB/)



+ [解释Java对象序列化](http://www.bcoder.top/2017/05/13/%E7%90%86%E8%A7%A3Java%E5%AF%B9%E8%B1%A1%E5%BA%8F%E5%88%97%E5%8C%96/)



+ [什么是内存泄漏？怎样解决？](http://www.bcoder.top/2017/03/22/JAVA%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E5%88%86%E6%9E%90/)


### java集合



+ [ArrayList、LinkedList、Vector的区别](https://github.com/zlnnjit/interview/blob/master/java/%C2%96ArrayList%E3%80%81LinkedList%E3%80%81Vector%E7%9A%84%E5%8C%BA%E5%88%AB.md)




+ [ArrayList如何实现扩容](https://github.com/zlnnjit/interview/blob/master/java/ArrayList%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%89%A9%E5%AE%B9.md)


+ [Collections常见问题总结](http://www.bcoder.top/2017/03/15/Collections%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93/)



### java多线程

+ [java有哪几种线程状态？如何转换？](https://github.com/zlnnjit/interview/blob/master/java/java%E6%9C%89%E5%93%AA%E5%87%A0%E7%A7%8D%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%EF%BC%9F%E5%A6%82%E4%BD%95%E8%BD%AC%E6%8D%A2%EF%BC%9F.md)


+ [进程和线程的区别,进程间如何通信？](https://github.com/zlnnjit/interview/blob/master/java/%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%8C%BA%E5%88%AB%2C%E8%BF%9B%E7%A8%8B%E9%97%B4%E5%A6%82%E4%BD%95%E9%80%9A%E4%BF%A1%EF%BC%9F.md)



+ [java线程如何通信](https://github.com/zlnnjit/interview/blob/master/java/java%E7%BA%BF%E7%A8%8B%E5%A6%82%E4%BD%95%E9%80%9A%E4%BF%A1.md)



<br><br><br><br>
## SDN篇
+ [Openflow1.3协议分析](http://www.bcoder.top/2017/09/01/OpenFlow1-3%E6%80%BB%E7%BB%93/)


<br><br><br><br>
## 数据库篇
### Redis
### MySQL



<br><br><br><br>
## javaweb篇



<br><br><br><br>
## 框架篇


<br><br><br><br>
## 数据结构篇



+ [二叉树](http://www.bcoder.top/2017/08/12/%C2%96%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E4%B9%8B%E4%BA%8C%E5%8F%89%E6%A0%91/)





<br><br><br><br>
## 算法篇



+ [八种常见的排序](http://www.bcoder.top/2017/08/06/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E4%B9%8B%E6%8E%92%E5%BA%8F/)

<br><br><br><br>
## 前端篇


<br><br><br><br>
## java设计模式篇




<br><br><br><br>
## java高级篇 

### java8

### NIO

### JUC

<br><br><br><br>
## Linux篇





<br><br><br><br>
## 工具篇

### Git

### SVN

### maven
<br><br><br><br>
## 高并发下的数据处理与数据库查询
<br><br><br><br>
## 高級篇

### SOA

### Dubbo

### JVM调优

### 数据库调优


### Spring Boot


### Nginx


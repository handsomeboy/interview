# 什么是ConcurrentHashMap,与HashTable有什么区别？


ConcurrentHashMap是在Java 1.5作为Hashtable的替代选择新引入的，是concurrent包的重要成员。在Java 1.5之前，如果想要实现一个可以在多线程和并发的程序中安全使用的Map,只能在HashTable和synchronized Map中选择，因为HashMap并不是线程安全的。但再引入了ConcurrentHashMap之后，我们有了更好的选择。

ConcurrentHashMap不但是线程安全的，而且比HashTable和synchronizedMap的性能要好。相对于HashTable和synchronizedMap锁住了整个Map，ConcurrentHashMap只锁住部分Map。ConcurrentHashMap允许并发的读操作，同时通过同步锁在写操作时保持数据完整性。
<br>

## 1. Java中ConcurrentHashMap如何实现？

ConcurrentHashMap引入了分割，并提供了HashTable支持的所有的功能。

在ConcurrentHashMap中，支持多线程对Map做读操作，并且不需要任何的blocking。这得益于ConcurrentHashMap将Map分割成了不同的部分，在执行更新操作时只锁住一部分。

根据默认的并发级别(concurrency level)，Map被分割成16个部分，并且由不同的锁控制。这意味着，同时最多可以有16个写线程操作Map。

试想一下，由只能一个线程进入变成同时可由16个写线程同时进入(读线程几乎不受限制)，性能的提升是显而易见的。

但由于一些更新操作，如put(),remove(),putAll(),clear()只锁住操作的部分，所以在检索操作不能保证返回的是最新的结果。


另一个重要点是在迭代遍历ConcurrentHashMap时，keySet返回的iterator是弱一致和fail-safe的，可能不会返回某些最近的改变，并且在遍历过程中，如果已经遍历的数组上的内容变化了，不会抛出ConcurrentModificationExceptoin的异常。

ConcurrentHashMap默认的并发级别是16，但可以在创建ConcurrentHashMap时通过构造函数改变。毫无疑问，并发级别代表着并发执行更新操作的数目，所以如果只有很少的线程会更新Map，那么建议设置一个低的并发级别。另外，ConcurrentHashMap还使用了ReentrantLock来对segments加锁。

Java中ConcurrentHashMap putifAbsent方法的例子

很多时候我们希望在元素不存在时插入元素，我们一般会像下面那样写代码
```java
synchronized(map){
  if (map.get(key) == null){
      return map.put(key, value);
  } else{
      return map.get(key);
  }
}
```

上面这段代码在HashMap和HashTable中是好用的，但在ConcurrentHashMap中是有出错的风险的。这是因为ConcurrentHashMap在put操作时并没有对整个Map加锁，所以一个线程正在put(k,v)的时候，另一个线程调用get(k)会得到null，这就会造成一个线程put的值会被另一个线程put的值所覆盖。当然，你可以将代码封装到synchronized代码块中，这样虽然线程安全了，但会使你的代码变成了单线程。ConcurrentHashMap提供的putIfAbsent(key,value)方法原子性的实现了同样的功能，同时避免了上面的线程竞争的风险。

<br>

## 2. 为什么我们需要ConcurrentHashMap和CopyOnWriteArrayList？

同步的集合类（Hashtable和Vector），同步的封装类（使用Collections.synchronizedMap()方法和Collections.synchronizedList()方法返回的对象）可以创建出线程安全的Map和List。但是有些因素使得它们不适合高并发的系统。它们仅有单个锁，对整个集合加锁，以及为了防止ConcurrentModificationException异常经常要在迭代的时候要将集合锁定一段时间，这些特性对可扩展性来说都是障碍。

ConcurrentHashMap和CopyOnWriteArrayList保留了线程安全的同时，也提供了更高的并发性。ConcurrentHashMap和CopyOnWriteArrayList并不是处处都需要用，大部分时候你只需要用到HashMap和ArrayList，它们用于应对一些普通的情况。

<br>

## 3. 什么时候使用ConcurrentHashMap?

ConcurrentHashMap适用于读者数量超过写者时，当写者数量大于等于读者时，ConcurrentHashMap的性能是低于Hashtable和synchronized Map的。这是因为当锁住了整个Map时，读操作要等待对同一部分执行写操作的线程结束。ConcurrentHashMap适用于做cache,在程序启动时初始化，之后可以被多个请求线程访问。正如Javadoc说明的那样，ConcurrentHashMap是HashTable一个很好的替代，但要记住，ConcurrentHashMap的比HashTable的同步性稍弱。

为什么我们需要ConcurrentHashMap和CopyOnWriteArrayList？

同步的集合类（Hashtable和Vector），同步的封装类（使用Collections.synchronizedMap()方法和Collections.synchronizedList()方法返回的对象）可以创建出线程安全的Map和List。但是有些因素使得它们不适合高并发的系统。它们仅有单个锁，对整个集合加锁，以及为了防止ConcurrentModificationException异常经常要在迭代的时候要将集合锁定一段时间，这些特性对可扩展性来说都是障碍。

ConcurrentHashMap和CopyOnWriteArrayList保留了线程安全的同时，也提供了更高的并发性。ConcurrentHashMap和CopyOnWriteArrayList并不是处处都需要用，大部分时候你只需要用到HashMap和ArrayList，它们用于应对一些普通的情况。

<br>

## 4. HashTable与ConcurrentHashMap的对比
HashTable本身是线程安全的，写过Java程序的都知道通过加Synchronized关键字实现线程安全，这样对整张表加锁实现同步的一个缺陷就在于使程序的效率变得很低。这就是为什么Java中会在1.5后引入ConcurrentHashMap的原因。

![什么是ConcurrentHashMap,与HashTable有什么区别？](http://www.bcoder.top/img/interview/10.jpg)

从图中可以看出，HashTable的锁加在整个Hash表上，而ConcurrentHashMap将锁加在segment上（每个段上），这样我们在对segment1操作的时候，同时也可以对segment2中的数据操作，这样效率就会高很多。



## 5. ConcurrentHashMap在jdk 1.6中的实现
ConcurrentHashMap采用**分段锁的机制**，实现并发的更新操作，底层采用**数组+链表+红黑树**的存储结构。

ConcurrentHashMap主要有三大结构：**整个Hash表，segment（段），HashEntry（节点）。每个segment就相当于一个HashTable。**

一个 ConcurrentHashMap 实例中包含由若干个 Segment 对象组成的数组，下面我们通过一个图来演示一下 ConcurrentHashMap 的结构：

![什么是ConcurrentHashMap,与HashTable有什么区别？](http://www.bcoder.top/img/interview/11.png)
<br>
**HashEntry类**

每个HashEntry代表Hash表中的一个节点，在其定义的结构中可以看到，除了value值没有定义final，其余的都定义为final类型，我们知道Java中关键词final修饰的域成为最终域。用关键词final修饰的变量一旦赋值，就不能改变，也称为修饰的标识为常量。这就意味着我们删除或者增加一个节点的时候，就必须从头开始重新建立Hash链，因为next引用值需要改变。
```
static final class HashEntry<K,V> { 
        final K key;                 // 声明 key 为 final 型
        final int hash;              // 声明 hash 值为 final 型 
        volatile V value;           // 声明 value 为 volatile 型
        final HashEntry<K,V> next;  // 声明 next 为 final 型 
 
        HashEntry(K key, int hash, HashEntry<K,V> next, V value)  { 
            this.key = key; 
            this.hash = hash; 
            this.next = next; 
            this.value = value; 
        } 
 }
 ```
由于这样的特性，所以插入Hash链中的数据都是从头开始插入的。例如将A,B,C插入空桶中，插入后的结构为：

![什么是ConcurrentHashMap,与HashTable有什么区别？](http://www.bcoder.top/img/interview/12.jpg)
<br>
**segment类**
Segment 类继承于 ReentrantLock 类，从而使得 Segment 对象能充当锁的角色。每个 Segment 对象用来守护其（成员对象 table 中）包含的若干个桶。

table 是一个由 HashEntry 对象组成的数组。table数组的每一个数组成员就是散列映射表的一个桶。

count 变量是一个计数器，它表示每个 Segment 对象管理的 table 数组（若干个 HashEntry 组成的链表）包含的 HashEntry 对象的个数。每一个 Segment 对象都有一个 count 对象来表示本 Segment 中包含的 HashEntry 对象的总数。注意，之所以在每个 Segment 对象中包含一个计数器，而不是在 ConcurrentHashMap 中使用全局的计数器，是为了避免出现“热点域”而影响 ConcurrentHashMap 的并发性。

```java
static final class Segment<K,V> extends ReentrantLock implements Serializable {  
 private static final long serialVersionUID = 2249069246763182397L;  
         /** 
          * 在本 segment 范围内，包含的 HashEntry 元素的个数
          * 该变量被声明为 volatile 型,保证每次读取到最新的数据
          */  
         transient volatile int count;  

         /** 
          *table 被更新的次数
          */  
         transient int modCount;  

         /** 
          * 当 table 中包含的 HashEntry 元素的个数超过本变量值时，触发 table 的再散列
          */  
         transient int threshold;  

         /** 
          * table 是由 HashEntry 对象组成的数组
          * 如果散列时发生碰撞，碰撞的 HashEntry 对象就以链表的形式链接成一个链表
          * table 数组的数组成员代表散列映射表的一个桶
          * 每个 table 守护整个 ConcurrentHashMap 包含桶总数的一部分
          * 如果并发级别为 16，table 则守护 ConcurrentHashMap 包含的桶总数的 1/16 
          */  
         transient volatile HashEntry<K,V>[] table;  

         /** 
          * 装载因子
          */  
         final float loadFactor;  
 }
```
<br>

**ConcurrentHashMap 类**

默认的情况下，每个ConcurrentHashMap 类会创建16个并发的segment，每个segment里面包含多个Hash表，每个Hash链都是有HashEntry节点组成的。

```java
public class ConcurrentHashMap<K, V> extends AbstractMap<K, V>  
         implements ConcurrentMap<K, V>, Serializable {  
     /** 
      * segments 的掩码值
      * key 的散列码的高位用来选择具体的 segment  
      */  
     final int segmentMask;  

     /** 
      * 偏移量
      */  
     final int segmentShift;  

     /** 
      * 由 Segment 对象组成的数组，每个都是一个特别的Hash Table
      */  
     final Segment<K,V>[] segments;  
 }
```

**jdk 1.8的实现已经抛弃了Segment分段锁机制，利用CAS+Synchronized来保证并发更新的安全，底层依然采用数组+链表+红黑树的存储结构**。


## 6. 总结
，下面我们来复习一下ConcurrentHashMap的一些关键点。

+ ConcurrentHashMap允许并发的读和线程安全的更新操作
+ 在执行写操作时，ConcurrentHashMap只锁住部分的Map
+ 并发的更新是通过内部根据并发级别将Map分割成小部分实现的
+ 高的并发级别会造成时间和空间的浪费，低的并发级别在写线程多时会引起线程间的竞争
+ ConcurrentHashMap的所有操作都是线程安全
+ ConcurrentHashMap返回的迭代器是弱一致性，fail-safe并且不会抛出ConcurrentModificationException异常
+ ConcurrentHashMap不允许null的键值
+ 可以使用ConcurrentHashMap代替HashTable，但要记住ConcurrentHashMap不会锁住整个Map









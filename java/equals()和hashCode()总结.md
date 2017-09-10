# equals()和hashCode()总结

首先，我们要明确一点，hashcode()方法和equals()方法都来源于Object类

## HashCode()作用

1、HashCode的存在主要是为了查找的快捷性，HashCode是用来在散列存储结构中确定对象的存储地址的 

2、如果两个对象equals相等，那么这两个对象的HashCode一定也相同 

3、如果对象的equals方法被重写，那么对象的HashCode方法也尽量重写 

4、如果两个对象的HashCode相同，不代表两个对象就相同，只能说明这两个对象在散列存储结构中，存放于同一个位置 

### 举个实际的例子Set

我们知道Set里面的元素是不可以重复的，那么如何做到？Set是根据equals()方法来判断两个元素是否相等的。

比方说Set里面已经有1000个元素了，那么第1001个元素进来的时候，最多可能调用1000次equals方法，如果equals方法写得复杂，对比的 东西特别多，那么效率会大大降低。

使用HashCode就不一样了，比方说HashSet，底层是基于HashMap实现的，先通过HashCode取一个模，这样一下子就固定到某个位置了，如果这个位置上没有元素，那么就可以肯定HashSet中必定没有和新添加的元素equals的元素，就可以直接存放了，都不需要比较；如果这个位置上有元素了，逐一比较，比较的时候先比较HashCode，HashCode都不同接下去都不用比了，肯定不一 样，HashCode相等，再equals比较，没有相同的元素就存，有相同的元素就不存。

如果原来的Set里面有相同的元素，只要HashCode的生成方式定义得好（不重复），不管Set里面原来有多少元素，只需要执行一次的equals就可以了。这样一来，实际调用equals方法的次数大大降低， 提高了效率。

## equals()作用

首先，我们来看看Object如何实现equals方法的
```java
public boolean equals(Object obj) {   
    return (this == obj);     
}  
```

也就是说，不重写equals方法的话，equals和==都表示相同的意思，即比较内存地址是否相同

默认情况下也就是从超类Object继承而来的equals方法与‘==’是完全等价的，比较的都是对象的内存地址，但我们可以重写equals方法，使其按照我们的需求的方式进行比较，如String类重写了equals方法，使其比较的是字符的序列，而不再是内存地址。


## 为什么重写equals()的同时还得重写hashCode()

![equals()和hashCode()总结](http://www.bcoder.top/img/interview/43.png)


从上面的分析过程我们能看出将一个（key，value）加入map中其实仅仅与key有关，而且在加入过程中首先使用到了hashcode然后使用了equals，所以说如果equals表示了两个对象的相等关系却没有保证其hashcode也相等就会出现在hashmap中加入了两个相等的key的情况，这也就是为什么在重写equals的同时一定要重写hashcode的根本原因。

如果研究get方法的源码也同样会发现与普通相似的处理算法。

不论是直接还是间接使用了HashMap HashTable（注意HashSet底层就是使用了一个HashMap），都必须在重写equals的同时重写hashcode。





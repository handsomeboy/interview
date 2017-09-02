# ArrayList如何实现扩容

---

## 集合内存分配以及初始化过程图解

![ArrayList如何实现扩容](http://www.bcoder.top/img/interview/1.jpg)


<br><br>
## 源码解读ArrayList内部实现（数组结构）

构造ArrayList的时候，默认初始化容量为10，保存容器为 Object[] elementData。
向集合添加元素的时候，调用add方法，比如list.add("a");
add方法做的操作是：elementData[size++] = e; 然后元素就被存放进了elementData。
初始化容量为10，当我们存第十一个元素的时候，会怎么做呢？看ArrayList类的部分源码：
```java
public class ArrayList<E> extends AbstractList<E> implements List<E> {  
    private transient Object[] elementData;  
      
    public ArrayList(int initialCapacity) {  
        super();  
        if (initialCapacity < 0)  
            throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);  
        this.elementData = new Object[initialCapacity];  
    }  
    //Constructs an empty list with an initial capacity of ten.  
    public ArrayList() {  
        this(10);  
    }  
    public boolean add(E e) {  
        ensureCapacity(size + 1);  // 扩充长度  
        elementData[size++] = e; // 先赋值，后进行size++。所以是从[0]开始存。  
        return true;  
    }  
    public void ensureCapacity(int minCapacity) {  
        modCount++;  
        int oldCapacity = elementData.length; // 旧集合长度  
        if (minCapacity > oldCapacity) {  
            Object oldData[] = elementData; // 旧集合数据  
            int newCapacity = (oldCapacity * 3)/2 + 1; // 计算新长度，旧长度的1.5倍+1  
                if (newCapacity < minCapacity)  
                    newCapacity = minCapacity;  
                // minCapacity is usually close to size, so this is a win:  
                elementData = Arrays.copyOf(elementData, newCapacity); // 这就是传说中的可变集合。用新长度复制原数组。  
        }  
    }  
    public E get(int index) {  
        RangeCheck(index);  
        return (E) elementData[index];  
    }  
}  

```

注意这句： elementData = Arrays.copyOf(elementData, newCapacity);

所以，具体的扩容过程为：
1.add方法中先调用**ensureCapacity方法**对原数组长度进行扩充，扩充方式为，通过Arrays类的copyOf方法对原数组进行拷贝，长度为原数组的**1.5倍+1**。

2.然后把扩容后的新数组实例对象地址赋值给elementData引用类型变量。扩容完毕。


为了自动扩容实现这一机制，java引进了Capacity和size概念，以区别数组的length。为了保证用户增加新的列表对象，java设置了最小容量（minCapacity）
，通常情况上，它大于列表对象的数目，所以Capactiy虽然就是底层数组的长度（length），但是对于最终用户来讲，它是无意义的。而size存储着列表
对象的数量，才是最终用户所需要的。为了防止用户错误修改，这一属性被设置为privae的，不过可以通过size()获取。

<br><br>
## 扩容性能简单分析

可以看到，扩容长度是旧长度的1.5倍+1，可以通过下面的程序计算出扩容次数：
```java
public class Test {  
    public static void main(String[] args) {  
        increse(10, 1000);  
    }  
  
    private static int newCapacity; // 新容量  
    private static int count; // 扩容次数  
    private static int temp;  
      
    /** 
     * 计算扩容次数 
     * @param init 集合初始化大小 
     * @param elementDataSize 元素个数 
     */  
    public static void increse(int init, int elementDataSize) {  
        temp = init;  
        while (true) {  
            if (newCapacity >= elementDataSize)  
                break;  
            increse(temp);  
        }  
    }  
    private static void increse(int init) {  
        newCapacity = (int) (init * 1.5 + 1); // 计算新容量  
        System.out.println("扩容第 [" + ++count + "] 次" + "\t"  
                                        + "新容量=" + newCapacity);  
        temp = newCapacity;  
    }  
} 
```
经测试，如果要存100万数据，需要扩容28次，数据量越大，扩容次数越多，每一次的扩容代表着创建新数组对象，复制原有数据。
如果数据很大，那么有必要为集合初始化一个默认大小，防止多次扩容，但如果数据增长很慢，那么就会浪费内存了，具体怎么做，还是要看实际应用场景。这里只做初步分析。




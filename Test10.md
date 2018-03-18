## 面试题10：二进制中1的个数


题目：输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

先安利一种错误解法：
```java
public static int NumberOf1_CanNotUse(int n) {
    int count = 0;
    while (n != 0) {
     /*
     * 用1和n进行位与运算，
     * 结果要是为1则n的2进制形式
     * 最右边那位肯定是1，否则为0
     */
        if ((n & 1) == 1) {
            count++;
        }
        //把n的2进制形式往右推一位
        n = n >> 1;
    }
    return count;
}
```
该解法如果输入时负数会陷入死循环，因为负数右移时，在最高位补得是1二本题最终目的是求1的个数，那么会有无数个1了。


第一种解法：
用1（1自身左移运算，其实后来就不是1了）和n的每位进行位与，来判断1的个数.

实现代码：
```java
public int NumberOf1(int n) {
    int count = 0;
    int flag = 1;
    while (flag != 0) {
        if ((n & flag) != 0) {
            count++;
        }
        flag = flag << 1;
    }
    return count;
}
```

再给出一种最优解：

思路是这样的：

如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。

举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

实现代码：
```java
public int NumberOf1(int n) {
    int count = 0;
    while (n != 0) {
        count++;
        n = n & (n - 1);
    }

    return count;
}
```

下面再给出两种最简单写法的解法(仅仅针对于java)：
```java
public int NumberOf1(int n) {
    return Integer.toBinaryString(n).replaceAll("0","").length();
}
```

```java
    public int NumberOf1(int n) {
       return Integer.bitCount(n);
    }

```



bitCount()方法的源码如下,有兴趣的小伙伴可以研究一下：

jdk8之前：
```java
public static int bitCount(int i) {
     // HD, Figure 5-2
     i = i - ((i >>> 1) & 0x55555555);
     i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
     i = (i + (i >>> 4)) & 0x0f0f0f0f;
     i = i + (i >>> 8);
     i = i + (i >>> 16);
     return i & 0x3f;
}
```

jdk8之后：
```java
public static int bitCount(int var0) {
    var0 -= var0 >>> 1 & 1431655765;
    var0 = (var0 & 858993459) + (var0 >>> 2 & 858993459);
    var0 = var0 + (var0 >>> 4) & 252645135;
    var0 += var0 >>> 8;
    var0 += var0 >>> 16;
    return var0 & 63;
}
```




## 面试题9：斐波那契数列

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。

教科书式的解法为递归：
```
    public int Fibonacci(int n) {
        if (n ==0){
            return 0;
        }
        if (n == 1) {
            return 1;
        }

        return Fibonacci(n-1)+Fibonacci(n-2);
    }
```

这种解法写起来比较简单，也好理解，问题是如果n比较大，不仅会导致重复计算的次数会增加，也会导致递归栈内存溢出，不怎么实用。

我们往往采用循环的方式地进行实现斐波那契数列：
```java
    public int Fibonacci(int n) {
        if (n <= 1) {
            return n;
        }

        int result = 0;
        int a=0,b=1;
        for(int i=2;i<=n;i++){
            result = a+b;
            a=b;
            b=result;
        }
        return result;
    }

```





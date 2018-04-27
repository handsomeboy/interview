# 面试题46：求1+2+3+...+n

题目：求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。


**解法一：用递归+异常**
实现代码：
```java
public int Sum_Solution(int n) {
    try {
        int i = 1 / n;
    }catch (Exception e){
        return 0;
    }
    return n + Sum_Solution(n - 1);
}
```
**解法二：用递归+短路**
实现代码：
```java
public int Sum_Solution(int n) {
    int sum = n;
    Boolean temp = n > 0 && ((sum += Sum_Solution(n - 1)) > 0);
    return sum;
}
```


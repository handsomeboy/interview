# 面试题52：构建乘积数组
题目：给定一个数组A[0,1,…,n-1],请构建一个数组B[0,1,…,n-1],其中B中的元素B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1],不能使用除法。

思路：先算左下三角中的连乘（从上往下），即我们先算出B[i]中的一部分，然后倒过来按右上三角中的分布规律，把另一部分也乘进去（从下到上）。

实现代码：
```java
public int[] multiply(int[] A) {
    int length = A.length;

    if (length > 0) {
        int[] B = new int[length];
        B[0] = 1;
        //计算左下三角
        for (int i=1;i<length;i++) {
            B[i] = B[i - 1] * A[i-1];
        }

        //计算右上三角
        int temp = 1;
        for (int i=length-2;i>-1;i--) {
            temp *=   A[i + 1];
            B[i] *= temp;
        }
        return B;
    }
    return null;
}

```





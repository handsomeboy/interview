## 面试题11：数值的整数次方

题目：实现函数double Power(double base,int exponent),求base的exponent次方。不得使用库函数，同时不需要考虑大数问题

先提供一种考虑不周全的解法：
```java
public double powerWithExponent(double base,int exponent){  
    double result = 1.0;  
    for(int i = 1;i<= exponent;i++){  
        result = result*base;  
    }  
    return result;  
}  

```

上面的代码，输入指数（exponent）小于1即0和负数的时候，将不能通过代码的测试用例。

对于这道题，要考虑四种情况： 
1.底数为0，指数为负数的情况，无意义 
2.指数为0，返回1 
3.指数为负数，返回1.0/base,-exponent 
4.指数正数，base,exponent 

由于0的0次方在数学上没有意义的，因此无论是输出0还是1都是可以接收的，但这都需要和面试官说清楚，表明我们已经考虑到了这个边界值了。同时因为我们输入的是double，由于计算机表示小数（包括float和double型小数）都会有误差，我们不能直接用等号（==）判断两个小数是否相等。如果两个小数的差的绝对值很小，比如小于0.0000001，就可以认为他们相等。

实现代码：
```java
public double Power(double base, int exponent) {
    double result = 0.0;

    if (equals(base,0.0)){
        return result;
    }

    if (exponent >= 0) {
        return PowerWithExponent(base, exponent);
    }

    if (exponent < 0) {
        return PowerWithExponent(1.0/base, -exponent);
    }

    return result;

}

private double PowerWithExponent(double base, int exponent) {
    double result = 1.0;
    for (int i=exponent;i>0;i--) {
        result *= base;
    }
    return result;
}

private boolean equals(double base, double v) {
    return base - v > 0 ? (base - v < 0.0000001) : (base - v > -0.0000001);
}
```

上面这种解法已经能够入得厅堂了，但是还是不够，我们需要分析一下PowerWithExponent这个函数，假如我们的目标是求出一个数字的32次方，如果我们已经知道了它的16次方，那么只要16次放的基础上再平方一次就可以了。而16次方又是8次方的平方。这样以此类推，我们求32次方只需要5次乘方：先求平方，在平方的基础上求4次方，在4次方的基础上求8次方，在8次方的基础上求16次方，最后在16此方的基础上求32次方。

我们可以利用下面的公式：
```
      |a^(n/2)*a^(n/2),n为偶数
a^n = |
      |a^(n-1/2)*a^(n-1/2)*a,n为奇数
```

PowerWithExponent这个方法改进版如下：
```
 private double PowerWithExponent(double base, int exponent) {
    if(exponent==0){
        return 1;
    }

    if(exponent==1){
        return base;
    }

    double result =  PowerWithExponent(base,exponent>>1);
    result *= result;
    if ((exponent&0x1) == 1) {
        return result * base;
    }
    return result;
}
```









# 面试题49：把字符串转换成整数

题目:将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0

思路：这看起来是很简单的题目，实现基本功能 ，大部分人都能用10行之内的代码解决。可是，当我们要把很多特殊情况即测试用例都考虑进去，却不是件容易的事。解决数值转换问题本身并不难，但我希望在写转换数值的代码之前，应聘者至少能把空指针，空字符串”“，正负号，溢出等方方面面的测试用例都考虑到，并且在写代码的时候对这些特殊的输入都定义好合理的输出

代码实现：
```java
public int StrToInt(String str) {
    if (str == null || str.length() < 1 || str.trim().length() < 1) {
        return 0;
    }

    int result = 0;
    int go = 10;

    if ((str.charAt(0) =='+'||str.charAt(0) =='-') && str.length() > 1) {
        for (int i=1;i<str.length();i++) {
            if (str.charAt(i) > '0' && str.charAt(i) < '9') {
                result = result * go + (str.charAt(i) - '0');

            }else {
                return 0;
            }
        }

        return str.charAt(0) =='+'?result:-result;
    }


    for (int i=0;i<str.length();i++) {
        if (str.charAt(i) > '0' && str.charAt(i) < '9') {
            result = result * go + (str.charAt(i) - '0');

        }else {
            return 0;
        }
    }

    return result;
}
```

需要注意的是，此代码并没有考虑int越界。






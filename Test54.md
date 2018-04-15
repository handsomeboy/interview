# 面试题54：表示数值的字符串

题目：请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串“+100”，“5e2”，“-123”，“3.1416”及”-1E-16”都表示数值，但“12e”,”1a3.14”,”1.2.3”,”+-5”及“12e+5.4”都不是。 

**解法一：扫描字符串**

思路：在数值之前可能有一个表示正负的’-‘或者’+’。接下来是若干个0到9的数位表示数值的整数部分（在某些小数里可能没有数值的整数部分）。如果数值是一个小数，那么在小数点后面可能会有若干个0到9的数位表示数值的小数部分。如果数值用科学计数法表示，接下来是一个’e’或者‘E’，以及紧跟着的一个整数（可以有正负号）表示指数。 
判断一个字符串是否符合上述模式时，首先看第一个字符是不是正负号。如果是，在字符串上移动一个字符，继续扫描剩余的字符串中0到9的数位。如果是一个小数，则将遇到小数点。另外，如果是用科学计数法表示的数值，在整数或者小数的后面还有可能遇到’e’或者’E’。

代码实现：
```java
public boolean isNumeric(char[] str) {
    if (str == null || str.length < 1) {
        return false;
    }
    int index = 0;
    if (str[index] == '+' || str[index] == '-') {
        index++;
    }

    //说明str中只包含+或者-
    if (index >= str.length) {
        return false;
    }

    index = findNoNum(str, index);

    if (index == str.length) {
        return true;
    }

    // 还未到字符串的末尾
    if (index < str.length) {
        //遇到小数点(小数点结束也符合)
        if (str[index] == '.') {
            index = findNoNum(str, ++index);
            if (index == str.length) {
                return true;
            }
            //还未到字符串结束位置
            if ((str[index] == 'e' || str[index] == 'E') && (index + 1 < str.length)) {
                return isExponential(++index, str);
            }
            return false;
        }
    }

    if ((str[index] == 'e' || str[index] == 'E') && (index + 1 < str.length)) {
        //还未到字符串结束位置
        return isExponential(++index, str);
    }
    return false;
}

private int findNoNum(char[] str, int index) {
    for (; index < str.length&&(str[index] >= '0' && str[index] <= '9'); index++) {
    }
    return index;
}

private boolean isExponential(int index, char[] str) {
    if (str[index] == '+' || str[index] == '-') {
        index++;
    }
    // 到达字符串的末尾，就返回false
    if (index >= str.length) {
        return false;
    }
    index = findNoNum(str, index);
    // 如果已经处理到了的数字的末尾就认为是正确的指数
    return index >= str.length;
}
```

**解法二：使用正则表达式（matchs方法）（参照上一题的思路）**
```java

public boolean isNumeric(char[] str) {
    String string = String.valueOf(str);
    return string.matches("[\\+-]?[0-9]*(\\.[0-9]*)?([eE][\\+-]?[0-9]+)?");
}
```

ps:这种还是有点小bug的，不过也能通过oj

**解法三：使用string转double函数**

```java
public boolean isNumeric(char[] str) {
    try {
        Double.parseDouble(new String(str));
        return true;
    } catch (Exception e) {
        return false;
    }
}
```

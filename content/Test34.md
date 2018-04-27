## 面试题34：丑数


题目：我们把只包含因子2、3 和5 的数称作丑数（Ugly Number）。求从小到大的顺序的第1500个丑数。例如6、8 都是丑数，但14 不是，它包含因子7。习惯上我们把1 当做第一个丑数。


**解法一：逐个判断每个数字是不是丑数的解法，直观但不够高效。**

实现代码如下：
```java
public int GetUglyNumber_Solution(int index) {
    if (index <= 0) {
        return 0;
    }
    int number = 0;
    int uglyNumber = 0;
    while (uglyNumber<index) {
        ++number;
        if (isUglyNumber(number)) {
            ++uglyNumber;
        }
    }

    return number;
}

private boolean isUglyNumber(int number) {
    while (number % 2 == 0) {
        number = number / 2;
    }
    while (number % 3 == 0) {
        number = number / 3;
    }
    while (number % 5 == 0) {
        number = number / 5;
    }
    return number == 1;
}
```
运行超时:您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。
此代码是正确的代码，但是并没有通过oj，oj提示：运行超时:您的程序未能在规定时间内运行结束，请检查是否循环有错或算法复杂度过大。

说明我们代码的时间复杂度太高，需要更优的算法来实现。

**第二种：创建数组保存已经找到丑数，用空间换时间的解法。**

根据丑数的定义， 丑数应该是另一个丑数乘以2、3 或者5 的结果（1除外）。因此我们可以创建一个数组，里面的数字是排好序的丑数，每一个丑数都是前面的丑数乘以2、3或者5得到的。

这种思路的关键在于怎样确保数组里面的丑数是排好序的。假设数组中已经有若干个丑数排好序后存放在数组中，并且把己有最大的丑数记做M，我们接下来分析如何生成下一个丑数。该丑数肯定是前面某一个丑数乘以2、3 或者5 的结果， 所以我们首先考虑把已有的每个丑数乘以2。在乘以2 的时钝能得到若干个小于或等于M 的结果。由于是按照顺序生成的，小于或者等于M 肯定己经在数组中了，我们不需再次考虑：还会得到若干个大于M 的结果，但我们只需要第一个大于M 的结果，因为我们希望丑数是按从小到大的顺序生成的，其他更大的结果以后再说。我们把得到的第一个乘以2 后大于M 的结果记为M2，同样，我们把已有的每一个丑数乘以3 和5，能得到第一个大于M 的结果M3 和M，那么下一个丑数应该是M2、M3 和M5这3个数的最小者。

前面分析的时候，提到把已有的每个丑数分别都乘以2、3 和5。事实上这不是必须的，因为已有的丑数是按顺序存放在数组中的。对乘以2而言， 肯定存在某一个丑数T2，排在它之前的每一个丑数乘以2 得到的结果都会小于已有最大的丑数，在它之后的每一个丑数乘以2 得到的结果都会太大。我们只需记下这个丑数的位置， 同时每次生成新的丑数的时候，去更新这个T2。对乘以3 和5 而言， 也存在着同样的T3和T5。


代码实现：
```java
public int GetUglyNumber_Solution(int index) {
    if (index <= 0) {
        return 0;
    }
    int[] uglyNumbers = new int[index];
    uglyNumbers[0] = 1;
    int p2 = 0;
    int p3 = 0;
    int p5 = 0;
    int uglyNumberIndex =1;
    while (uglyNumberIndex<index) {

        int cur = Math.min(uglyNumbers[p2] * 2, Math.min(uglyNumbers[p3] * 3, uglyNumbers[p5] * 5));
        uglyNumbers[uglyNumberIndex] = cur;
        if (uglyNumbers[p2] * 2 == cur) {
            p2++;
        }
        if (uglyNumbers[p3] * 3 == cur) {
            p3++;
        }
        if (uglyNumbers[p5] * 5 == cur) {
            p5++;
        }
        uglyNumberIndex++;
    }

    return uglyNumbers[uglyNumberIndex-1];
}
```






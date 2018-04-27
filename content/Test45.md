# 面试题45：圆圈中最后剩下的数字(约瑟夫环问题)


题目：0, 1, … , n-1 这n个数字排成一个圈圈，从数字0开始每次从圆圏里删除第m个数字。求出这个圈圈里剩下的最后一个数字。


**解法一：经典的解法，用环形链表模拟圆圈。** 
创建一个总共有n 个结点的环形链表，然后每次在这个链表中删除第m个结点。我们这里用LinkedList模拟链表，用取模模拟循环链表。


代码实现：
```java
public int LastRemaining_Solution(int n, int m) {
    if (n<=0||m<=0) {
        return -1;
    }
    LinkedList<Integer> list = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        list.add(i);
    }
    int index = 0;
    while (list.size() > 1) {
        index = (index + m - 1) % list.size();
        list.remove(index);
    }
    return list.get(0);
}

```


我们发现使用环形链表里重复遍历很多遍。重复遍历当然对时间效率有负面的影响。这种方法每删除一个数字需要m步运算，总共有n个数字，因此总的时间复杂度为O(mn)。同时这种思路还需要一个辅助的链表来模拟圆圈，其空间复杂度O(n）。

**解法二：公式法（装逼点说叫动态规划）**
这种方法不易想到，能直接想到的要不就是见过类似的题，要不就是真的思维帝。


链接：https://www.nowcoder.com/questionTerminal/f78a359491e64a50bce2d89cff857eb6
来源：牛客网

问题的关键在于把公式推导出来，也就是剩下i - 1个人时，他们的编号是0到i - 2号，那么在上一轮还有i个人时他们的编号各是多少呢？假设m是小于i的（如果m >= i，就令m对i取模），那么

0号是上一轮的m号，
1号是上一轮的m + 1号，
2号是上一轮的m + 2号，
。。。。
p号是上一轮的m + p号

注意m + p 可能会大于或等于 i ，所以m + p 应该对 i 取模。

设F(i)是最后剩下的那个人在剩下i个人时的编号，那么问题的结果就是F(n)的值。公式是：
F(i) = (m + F(i - 1)) % i
只剩一个人时，他一定是0号，所以初始条件是
F(1) = 0

实现代码：
```java
public int LastRemaining_Solution(int n, int m) {
    if (n<=0||m<=0) {
        return -1;
    }
    int last = 0;
    for (int i=2;i<=n;i++) {
        last = (last + m) % i;
    }
    return last;
}
```
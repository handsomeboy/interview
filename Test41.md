## 面试题41：和为s的两个数字vs和为s的连续正数序列

题目一：输入一个递增排序的数组和一个数字s，在数组中查找两个数，得它们的和正好是s。如果有多对数字的和等于s，输出任意一对即可。

思路：我们先在数组中选择两个数字，如果它们的和等于输入的s，我们就找到了要找的两个数字。如果和小于s 呢？我们希望两个数字的和再大一点。由于数组已经排好序了，我们可以考虑选择较小的数字后面的数字。因为排在后面的数字要大一些，那么两个数字的和也要大一些， 就有可能等于输入的数字s 了。同样， 当两个数字的和大于输入的数字的时候，我们可以选择较大数字前面的数字，因为排在数组前面的数字要小一些。这种算法的时间复杂度是O(n).

这里不得不提一句，牛客网上题目是返回乘积最小的一组，其实这句话不影响我们的代码，因为外圈符合条件的肯定乘积最小。

实现代码：
```java
public ArrayList<Integer> FindNumbersWithSum(int[] array, int sum) {
    ArrayList<Integer> list = new ArrayList<>();
    if (array == null || array.length < 2) {
        return list;
    }
    int p1 = 0;
    int p2 = array.length - 1;
    while (p1 < p2) {
        if (array[p1] + array[p2] == sum) {
            list.add(array[p1++]);
            list.add(array[p2++]);
            return list;
        } else if (array[p1] + array[p2] > sum) {
            p2--;
        } else {
            p1++;
        }
    }
    return list;
}

```
题目二：输入一个正数s，打印出所有和为s的连续正数序列（至少两个数）。例如输入15，由于1+2+3+4+5=4＋5+6＝7+8=15，所以结果打出3 个连续序列1～5、4～6 和7～8。(牛客网输出：输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序，本次代码按照牛客网要求输出)


思路：考虑用两个数small 和big 分别表示序列的最小值和最大值。首先把small 初始化为1, big 初始化为2。如果从small 到big 的序列的和大于s，我们可以从序列中去掉较小的值，也就是增大small 的值。如果从small 到big 的序列的和小于s，我们可以增大big，让这个序列包含更多的数字。因为这个序列至少要有两个数字，我们一直增加small 到(1+s)/2 为止。 

实现代码：
```java
public ArrayList<ArrayList<Integer>> FindContinuousSequence(int sum) {
    ArrayList<ArrayList<Integer>> lists = new ArrayList<>();
    if (sum < 3) {
        return lists;
    }
    int p1 = 1;
    int p2 = 2;
    while (p1 < (sum + 1) / 2 && p2 < sum) {
        int curSum = sum(p1, p2);
        if (curSum == sum) {
            ArrayList<Integer> temp = buildList(p1, p2);
            lists.add(temp);
            p1++;
        } else if (curSum > sum) {
            p1++;
        } else {
            p2++;
        }
    }
    return lists;
}

private ArrayList<Integer> buildList(int p1, int p2) {
    ArrayList<Integer> list = new ArrayList<>();
    for (int i = p1; i <= p2; i++) {
        list.add(i);
    }
    return list;
}

public int sum(int p1, int p2) {
    int sum = 0;
    for (int i = p1; i <= p2; i++) {
        sum += i;
    }
    return sum;
}
```



# 面试题51：数组中重复的数字


题目：在一个长度为n的数组里的所有数字都在0到n-1的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是重复的数字2或者3。(牛客网上要求找出第一个重复的数，按照这个来)

**解法一：排序**

先把输入的数组排序。从排序的数组中找出重复的数字时间很容易的事情，只需要从头到尾扫描排序后的数组就可以了。排序一个长度为n的数组需要**O(nlogn)**的时间。 

**解法二：哈希表**
从头到尾按顺序扫描数组的每个数，每扫描一个数字的时候，都可以用O(1)的时间来判断哈希表里是否已经包含了该数字。如果哈希表里还没有这个数字，就把它加入到哈希表里。如果哈希表里已经存在该数字了，那么就找到一个重复的数字。这个算法的时间复杂度是O(n)，但它提高时间效率是以一个大小为**O(n)**的哈希表为代价的。 
在java中，我们可以使用HashSet和HashMap来进行构建hash表。

实现代码：
```java
public boolean duplicate(int numbers[],int length,int [] duplication) {
    HashSet<Integer> hashSet = new HashSet<>();
    for (int i=0;i<length;i++) {
        if (hashSet.contains(numbers[i])) {
            duplication[0] = numbers[i];
            return true;
        }
        hashSet.add(numbers[i]);
    }
    return false;
}
```
返回的boolean是指是否存在该数字，而duplication[0]表示第一个重复的数。

**解法三：桶排序**
由于数据范围固定，可以使用桶排序，时间复杂度也是**O(n)**.

```java

public boolean duplicate(int numbers[], int length, int[] duplication) {
    boolean[] k = new boolean[length];
    for (int i = 0; i < k.length; i++) {
        if (k[numbers[i]] == true) {
            duplication[0] = numbers[i];
            return true;
        }
        k[numbers[i]] = true;
    }
    return false;
}
```


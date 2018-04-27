## 面试题38：数字在排序数组中出现的次数

题目：统计一个数字：在排序数组中出现的次数。例如输入排序数组｛ 1, 2, 3, 3, 3, 3, 4, 5｝和数字3 ，由于3 在这个数组中出现了4 次，因此输出4 。


既然输入的数组是排序的，那么我们很自然的想到利用二分查找算法。在题目给出的例子中，我们可以先用二分查找算法找到第一个3.由于3可能出现多次，因此我们找到的3的左右两遍可能都是3，于是我们在找到3的左右两边顺序扫描，分别找出第一个3和最后一个3.因为要查找的数字在长度为n的数组中可能很出现O(n)次，所以顺序扫描的时间复杂度为**O(n)**。因此这种算法的效率和直接从头到尾顺序扫描整个数组统计3出现的次数的方法是一样的。


**解法：改进的二分查找(时间复杂度（logn）)** 

如何用二分查找算法在数组中找到第一个k，二分查找算法总是先拿数组中间的数字和k作比较。如果中间的数字比k大，那么k只有可能出现在数组的前半段，下一轮我们只在数组的前半段查找就可以了。如果中间的数字比k小，那么k只有可能出现在数组的后半段，下一轮我们只在数组的后半乓查找就可以了。如果中间的数字和k相等呢？我们先判断这个数字是不是第一个k。如果位于中间数字的前面一个数字不是k,此时中间的数字刚好就是第一个k。如果中间数字的前面一个数字也是k，也就是说第一个k肯定在数组的前半段， 下一轮我们仍然需要在数组的前半段查找。 

同样的思路在排序数组中找到最后一个k。如果中间数字比k大，那么k只能出现在数组的前半段。如果中间数字比k小，k就只能出现在数组的后半段。如果中间数字等于k呢？我们需要判断这个k是不是最后一个k，也就是中间数字的下一个数字是不是也等于k。如果下一个数字不是k，则中间数字就是最后一个k了：否则下一轮我们还是要在数组的后半段中去查找。


实现代码：
```java
public int GetNumberOfK(int[] array, int k) {
    int first = GetFirstK(array, k, 0, array.length - 1);
    int last = GetLastK(array, k, 0, array.length - 1);

    if (first != -1 && last != -1) {
        return last - first + 1;
    }
    return 0;
}


//找首次出现的K的索引
private int GetFirstK(int[] array, int k, int start, int end) {
    if (start > end) {
        return -1;
    }

    int middleIndex =  ((end - start) >> 1) + start;
    int middleData = array[middleIndex];

    if (middleData == k) {
        if ((middleIndex > 0 && array[middleIndex - 1] != k) || middleIndex == 0) {
            return middleIndex;
        } else {
            end = middleIndex - 1;
        }
    } else if (middleIndex > k) {
        end = middleIndex - 1;
    } else {
        start = middleIndex + 1;
    }

    return GetFirstK(array, k, start, end);
}

//找最后出现K的索引
private int GetLastK(int[] array, int k, int start, int end) {
    if (start > end) {
        return -1;
    }

    int middleIndex = ((end - start) >> 1) + start;
    int middleData = array[middleIndex];

    if (middleData == k) {
        if ((middleIndex < array.length-1 && array[middleIndex + 1] != k) || middleIndex == array.length-1) {
            return middleIndex;
        } else {
            start = middleIndex + 1;
        }
    } else if (middleIndex < k) {
        start = middleIndex + 1;
    } else {
        end = middleIndex - 1;
    }

    return GetLastK(array, k, start, end);

}
```

测试代码：
```java
@Test
public void test() {
    // 查找的数字出现在数组的中间
    int[] data1 = {1, 2, 3, 3, 3, 3, 4, 5};
    System.out.println(GetNumberOfK(data1, 3)); // 4

    // 查找的数组出现在数组的开头
    int[] data2 = {3, 3, 3, 3, 4, 5};
    System.out.println(GetNumberOfK(data2, 3)); // 4

    // 查找的数组出现在数组的结尾
    int[] data3 = {1, 2, 3, 3, 3, 3};
    System.out.println(GetNumberOfK(data3, 3)); // 4

    // 查找的数字不存在
    int[] data4 = {1, 3, 3, 3, 3, 4, 5};
    System.out.println(GetNumberOfK(data4, 2)); // 0

    // 查找的数字比第一个数字还小，不存在
    int[] data5 = {1, 3, 3, 3, 3, 4, 5};
    System.out.println(GetNumberOfK(data5, 0)); // 0

    // 查找的数字比最后一个数字还大，不存在
    int[] data6 = {1, 3, 3, 3, 3, 4, 5};
    System.out.println(GetNumberOfK(data6, 0)); // 0

    // 数组中的数字从头到尾都是查找的数字
    int[] data7 = {3, 3, 3, 3};
    System.out.println(GetNumberOfK(data7, 3)); // 4

    // 数组中的数字从头到尾只有一个重复的数字，不是查找的数字
    int[] data8 = {3, 3, 3, 3};
    System.out.println(GetNumberOfK(data8, 4)); // 0

    // 数组中只有一个数字，是查找的数字
    int[] data9 = {3};
    System.out.println(GetNumberOfK(data9, 3)); // 1

    // 数组中只有一个数字，不是查找的数字
    int[] data10 = {3};
    System.out.println(GetNumberOfK(data10, 4)); // 0
}

```

result:
```java
4
4
4
0
0
0
4
0
1
0
```
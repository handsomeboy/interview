## 面试题8 : 旋转数组的最小数字



题目： 把一个数组最开始的若干个元素搬到数组的末尾，我们称之数组的旋转。输入一个递增排序的数组的一个旋转， 输出旋转数组的最小元素。例如数组{3，4, 5, 1,2｝为｛1,2,3,4,5}的一个旋转，该数组的最小值为1(给出的所有元素都大于0，若数组大小为0，请返回0。)


先来一种O(n)的解法，用这种解法的话，面试基本凉了，哈哈。
```java
public class Test08 {
    public int minNumberInRotateArray(int [] array) {
        if (array.length == 0) {
            return 0;
        }

        int min = array[0];

        for (int i = 1; i < array.length; i++) {
            if (array[i] < min) {
                min = array[i];
            }
        }
        return min;
    }
}

```

下面来一种O(logn)的解法：
这种解法就是二分查找的变形：

旋转之后的数组实际上可以划分成两个有序的子数组：前面子数组的大小都大于后面子数组中的元素。

注意到实际上最小的元素就是两个子数组的分界线。本题目给出的数组一定程度上是排序的，因此我们试着用二分查找法寻找这个最小的元素。

思路：
（1）我们用两个指针left,right分别指向数组的第一个元素和最后一个元素。按照题目的旋转的规则，第一个元素应该是大于最后一个元素的（没有重复的元素）。
但是如果不是旋转，第一个元素肯定小于最后一个元素。

（2）找到数组的中间元素。
中间元素大于第一个元素，则中间元素位于前面的递增子数组，此时最小元素位于中间元素的后面。我们可以让第一个指针left指向中间元素。
移动之后，第一个指针仍然位于前面的递增数组中。
中间元素小于第一个元素，则中间元素位于后面的递增子数组，此时最小元素位于中间元素的前面。我们可以让第二个指针right指向中间元素。
移动之后，第二个指针仍然位于后面的递增数组中。
这样可以缩小寻找的范围。

（3）按照以上思路，第一个指针left总是指向前面递增数组的元素，第二个指针right总是指向后面递增的数组元素。
最终第一个指针将指向前面数组的最后一个元素，第二个指针指向后面数组中的第一个元素。
也就是说他们将指向两个相邻的元素，而第二个指针指向的刚好是最小的元素，这就是循环的结束条件。
到目前为止以上思路很耗的解决了没有重复数字的情况，这一道题目添加上了这一要求，有了重复数字。

因此这一道题目比上一道题目多了些特殊情况：
我们看一组例子：｛1，0，1，1，1｝ 和 ｛1，1， 1，0，1｝ 都可以看成是递增排序数组｛0，1，1，1，1｝的旋转。
这种情况下我们无法继续用上一道题目的解法，去解决这道题目。因为在这两个数组中，第一个数字，最后一个数字，中间数字都是1。
第一种情况下，中间数字位于后面的子数组，第二种情况，中间数字位于前面的子数组。
因此当两个指针指向的数字和中间数字相同的时候，我们无法确定中间数字1是属于前面的子数组还是属于后面的子数组。
也就无法移动指针来缩小查找的范围。

实现代码：
```java

public int minNumberInRotateArray(int[] array) {

    // 判断输入是否合法
    if (array == null ||  array.length == 0) {
        return 0;
    }

    // 开始处理的第一个位置
    int ps = 0;

    // 开始处理的最后一个位置
    int pe = array.length - 1;

    //开始处理的中间位置
    int pm = 0;

    // 确保ps在前一个排好序的部分，pe在排好序的后一个部分
    while (array[ps] >= array[pe]) {

        // 当处理范围只有两个数据时，返回后一个结果
        if (pe - ps == 1) {
            return array[pe];
        }

        // 如果三个数都相等，则需要进行顺序处理，从头到尾找最小的值
        if (array[pe] == array[pm] && array[ps] == array[pm]) {
            return find(array, ps, pe);
        }
        pm = ps + (pe - ps) / 2;

        if (array[pm] >= array[ps]) {
            ps = pm;
        }

        if (array[pm] <= array[pe]) {
            pe = pm;
        }

    }

    return array[ps];
}

private int find(int[] array, int start, int end) {
    int min = array[start];
    for (int i = start + 1; i <= end; i++) {
        if (min > array[i]) {
            min = array[i];
        }
    }
    return min;
}

```

测试代码：
```java
@Test
public void test() {
    // 典型输入，单调升序的数组的一个旋转
    int[] array1 = {3, 4, 5, 1, 2};
    System.out.println(minNumberInRotateArray(array1));

    // 有重复数字，并且重复的数字刚好的最小的数字
    int[] array2 = {3, 4, 5, 1, 1, 2};
    System.out.println(minNumberInRotateArray(array2));

    // 有重复数字，但重复的数字不是第一个数字和最后一个数字
    int[] array3 = {3, 4, 5, 1, 2, 2};
    System.out.println(minNumberInRotateArray(array3));

    // 有重复的数字，并且重复的数字刚好是第一个数字和最后一个数字
    int[] array4 = {1, 0, 1, 1, 1};
    System.out.println(minNumberInRotateArray(array4));

    // 单调升序数组，旋转0个元素，也就是单调升序数组本身
    int[] array5 = {1, 2, 3, 4, 5};
    System.out.println(minNumberInRotateArray(array5));

    // 数组中只有一个数字
    int[] array6 = {2};
    System.out.println(minNumberInRotateArray(array6));

    // 数组中数字都相同
    int[] array7 = {1, 1, 1, 1, 1, 1, 1};
    System.out.println(minNumberInRotateArray(array7));
    System.out.println(minNumberInRotateArray(array6));

    // 输入NULL
    System.out.println(minNumberInRotateArray(null));


}
```

运行结果：
```
1
1
1
0
1
2
1
2
0
```


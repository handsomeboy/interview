## 面试题36：数组中的逆序对


题目：在数组中的两个数字如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。例如在数组｛7, 5, 6, 4 ]中， 一共存在5 个逆序对，分别是（7, 6）、（7，5），(7, 4）、（6, 4）和（5, 4）。(牛客网中要求输出P%1000000007，其中P为总数)



**解法一：直接求解**

顺序扫描整个数组。每扫描到一个数字的时候，逐个比较该数字和它后面的数字的大小。如果后面的数字比它小，则这两个数字就组成了一个逆序对。假设数组中含有n 个数字。由于每个数字都要和O(n）个数字作比较， 因此这个算法的时间复杂度是O(n^2)。

实现代码：
```java
public int InversePairs(int [] array) {
    int p = 0;
    for (int i=0;i<array.length-1;i++) {
        for (int j=i+1;j<array.length;j++) {
            if (array[i] > array[j]) {
                p++;
            }
        }
    }
    return p%1000000007;
}
```

解法二：归并排序变种

本质是在并归排序拷贝数据到新数组的时候，计算逆序对的个数。

代码实现：
```java
private int p = 0;

public int InversePairs(int[] array) {
    mergeSort(array, 0, array.length - 1);
    return p % 1000000007;
}

public void merge(int[] a, int low, int mid, int high) {
    int[] temp = new int[high - low + 1];
    int i = low;// 左指针
    int j = mid + 1;// 右指针
    int k = 0;
    // 把较小的数先移到新数组中
    while (i <= mid && j <= high) {
        if (a[i] < a[j]) {
            temp[k++] = a[i++];

        } else {
            temp[k++] = a[j++];

            //数值过大求余
            if (p >= 1000000007) {
                p %= 1000000007;
            }
            p += mid - i + 1;
        }
    }
    // 把左边剩余的数移入数组
    while (i <= mid) {
        temp[k++] = a[i++];

    }
    // 把右边边剩余的数移入数组
    while (j <= high) {
        temp[k++] = a[j++];
    }
    // 把新数组中的数覆盖nums数组
    for (int k2 = 0; k2 < temp.length; k2++) {
        a[k2 + low] = temp[k2];
    }
}

public void mergeSort(int[] a, int low, int high) {
    int mid = (low + high) / 2;
    if (low < high) {
        // 左边
        mergeSort(a, low, mid);
        // 右边
        mergeSort(a, mid + 1, high);
        // 左右归并
        merge(a, low, mid, high);
    }

}
```

在测试oj的时候终于明白牛客网为啥要设置p%1000000007，因为测试样本比较大，p出现的次数可能会int类型溢出，因此在这里进行对一个大数取模，如果不进行取模的话，只能通过50%的数据。
  这里也建议再复习一下并归排序的实现、复杂度、稳定性等等。

## 面试题14：调整数组顺序使奇数位于偶数前面


题目:输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位予数组的后半部分。


这个题目要求把奇数放在数组的前半部分， 偶数放在数组的后半部分，因此所有的奇数应该位于偶数的前面。也就是说我们在扫描这个数组的时候， 如果发现有偶数出现在奇数的前面，我们可以交换它们的顺序，交换之后就符合要求了。

因此我们可以维护两个指针，第一个指针初始化时指向数组的第一个数字，它只向后移动：第二个指针初始化时指向数组的最后一个数字， 它只向前移动。在两个指针相遇之前，第一个指针总是位于第二个指针的前面。如果第一个指针指向的数字是偶数，并且第二个指针指向的数字是奇数，我们就交换这两个数字。

实现代码：
```
public void reOrderArray(int [] array) {
    if (array==null||array.length<=1){
        return ;
    }
    int start = 0;
    int end = array.length - 1;
    while (start != end) {
        if (array[start] % 2 == 0) {
            int temp = array[start];
            array[start] = array[end];
            array[end] = temp;
            end--;
        }else {
            start++;
        }
    }
}
```

测试代码：
```java
public  void printArray(int[] arr) {
    if (arr != null && arr.length > 0) {
        for (int i : arr) {
            System.out.print(i + " ");
        }
        System.out.println();
    }
}

@Test
public void test() {
    int[] array = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    reOrderArray(array);
    printArray(array);
}
```

result:
```
9 1 7 3 5 6 4 8 2 0
```



本来想将实现代码提交到牛客网的oj上额，后来发现牛客网上是本题的变种题，题目是这样的：
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

变种题也很简单，设置两个指针，一个是已经排好偶数的指针，另一个是往前面前进的探索只能，第二个指针遍历完成，数组满足要求。

实现代码：
```java
public void reOrderArray(int[] array) {
    if (array == null || array.length <= 1) {
        return;
    }
    int p = 0, q = 0;
    while (p < array.length) {
        if (array[p] % 2 == 1) {
            for (int i = p; i > q; i--) {
                swap(i, i - 1, array);
            }
            q++;
        }
        p++;

    }
}

private void swap(int start, int end, int[] array) {
    int temp = array[start];
    array[start] = array[end];
    array[end] = temp;
}
```

result:

```
1 3 5 7 9 0 2 4 6 8
```
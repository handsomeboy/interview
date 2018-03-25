## 面试题24：二叉搜索树的后序遍历序列


题目：输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则返回true。否则返回false。假设输入的数组的任意两个数字都互不相同。

思路：在后序遍历得到的序列中， 最后一个数字是树的根结点的值。数组中前面的数字可以分为两部分： 第一部分是左子树结点的值，它们都比根结点的值小： 第二部分是右子树结点的值，它们都比根结点的值大。


```java
public boolean VerifySquenceOfBST(int[] sequence) {
    // 输入的数组不能为空，并且有数据
    if (sequence == null || sequence.length < 1) {
        return false;
    }

    // 有数据，就调用辅助方法
    return VerifySquenceOfBST(sequence, 0, sequence.length - 1);
}

/**
 *
 * @param sequence 某二叉搜索树的后序遍历的结果
 * @param start    处理的开始位置
 * @param end      处理的结束位置
 * @return true：该数组是某二叉搜索树的后序遍历的结果。false：不是
 */
private boolean VerifySquenceOfBST(int[] sequence, int start, int end) {

    // 如果对应要处理的数据只有一个或者已经没有数据要处理（start>end）就返回true
    if (start >= end) {
        return true;
    }

    // 从左向右找第一个不大于根结点（sequence[end]）的元素的位置
    int index = start;
    while (index <= end - 1 && sequence[index] < sequence[end]) {
        index++;
    }

    for (int i = index; i < end; i++) {
        if (sequence[i] <= sequence[end]) {
            return false;
        }
    }

    return VerifySquenceOfBST(sequence, start, index - 1) && VerifySquenceOfBST(sequence, index, end - 1);
}

```

测试代码：
```java
@Test
public void test() {
    //           10
    //         /   \
    //        6     14
    //       /\     /\
    //      4  8  12  16
    int[] data = {4, 8, 6, 12, 16, 14, 10};
    System.out.println("true: " + VerifySquenceOfBST(data));

    //           5
    //          / \
    //         4   7
    //            /
    //           6
    int[] data2 = {4, 6, 7, 5};
    System.out.println("true: " + VerifySquenceOfBST(data2));

    //               5
    //              /
    //             4
    //            /
    //           3
    //          /
    //         2
    //        /
    //       1
    int[] data3 = {1, 2, 3, 4, 5};
    System.out.println("true: " + VerifySquenceOfBST(data3));

    // 1
    //  \
    //   2
    //    \
    //     3
    //      \
    //       4
    //        \
    //         5
    int[] data4 = {5, 4, 3, 2, 1};
    System.out.println("true: " + VerifySquenceOfBST(data4));

    // 树中只有1个结点
    int[] data5 = {5};
    System.out.println("true: " + VerifySquenceOfBST(data5));

    int[] data6 = {7, 4, 6, 5};
    System.out.println("false: " + VerifySquenceOfBST(data6));

    int[] data7 = {4, 6, 12, 8, 16, 14, 10};
    System.out.println("false: " + VerifySquenceOfBST(data7));
}
```

result:
```
true: true
true: true
true: true
true: true
true: true
false: false
false: false
```

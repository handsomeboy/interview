## 面试题28 ：字符串的排列

题目：输入一个字符串，打印出该字符串中字符的所有排列。例如输入字符串abc。则打印出由字符a、b、c 所能排列出来的所有字符串abc、acb、bac 、bca、cab 和cba 。


思路：

把一个字符串看成由两部分组成：第一部分为它的第一个字符，第二部分是后面的所有字符。在图4.14 中，我们用两种不同的背景颜色区分字符串的两部分。

我们求整个字符串的排列，可以看成两步：首先求所有可能出现在第一个位置的字符，即把第一个字符和后面所有的字符交换。图4.14 就是分别把第一个字符a 和后面的b、c 等字符交换的情形。首先固定第一个字符（如图4.14 (a ）所示〉，求后面所有字符的排列。这个时候我们仍把后面的所有字符分成两部分：后面字符的第一个字符，以及这个字符之后的所有字符。然后把第一个字符逐一和它后面的字符交换:

```java
public ArrayList<String> Permutation(String str) {
    ArrayList<String> list = new ArrayList<>();
    if (str == null || str.length() < 1) {
        return list;
    }
    char[] chars = str.toCharArray();

    fullArray(list, chars, 0, str.length() - 1);

    return list;
}

private void fullArray(ArrayList<String> list, char[] chars, int start, int end) {
    if (start == end) {
        list.add(new String(chars));
    } else {
        for (int i = start; i <= end; i++) {
            if (!swapAccepted(chars, start, i)) {
                continue;
            }
            swap(chars, start, i);
            fullArray(list, chars, start + 1, end);
            swap(chars, i, start); // 用于对之前交换过的数据进行还原
        }
    }
    

}

private static boolean swapAccepted(char[] chars, int start, int end) {
    for (int i = start; i < end; i++) {
        if (chars[i] == chars[end]) {
            return false;
        }
    }
    return true;
}
private void swap(char[] chars, int i, int j) {
    char temp = chars[i];
    chars[i] = chars[j];
    chars[j] = temp;
}
```

需要注意的是，此题这样解法没有通过牛客网的oj,牛客网的oj是将list的元素逐一比对，不一样就报错，而本题的全排列是无序的，因此应该算作牛客网的bug。估计牛客网是又重新排序了list，比较简单的AC的方法就是在 return list;前面加上：
```java
Collections.sort(list);
```






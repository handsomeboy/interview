## 面试题42：翻转单词顺序vs左旋转字符串

题目一：输入一个英文句子，翻转句子中单词的顺序，但单词内字啊的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串”I am a student. ”，则输出”student. a am I”。


思路:第一步翻转句子中所有的字符。比如翻转“I am a student. ”中所有的字符得到”.tneduts a m a I”，此时不但翻转了句子中单词的顺序，连单词内的字符顺序也被翻转了。第二步再翻转每个单词中字符的顺序，就得到了”student. a am I”。这正是符合题目要求的输出。

代码实现:
```java
public String ReverseSentence(String str) {
    if (str == null || str.length() < 2||str.trim().length()<1) {
        return str;
    }
    char[] chs = str.toCharArray();
    Reverse(chs, 0, chs.length - 1);
    int start = 0;

    for (int i = 1;i<chs.length;i++) {
        if (chs[i] == ' ') {
            Reverse(chs, start, i - 1);
            start = i + 1;
        }
    }
    if (start < chs.length - 1) {
        Reverse(chs, start, chs.length - 1);
    }
    return new String(chs);
}

public void Reverse(char[] chs, int start, int end) {
    while (start < end) {
        char temp = chs[start];
        chs[start] = chs[end];
        chs[end] = temp;
        start += 1;
        end -= 1;
    }
}
```

题目二：字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。题目二：字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。


思路:以”abcdefg”为例，我们可以把它分为两部分。由于想把它的前两个字符移到后面，我们就把前两个字符分到第一部分，把后面的所有字符都分到第二部分。我们先分别翻转这两部分，于是就得到”bagfedc”。接下来我们再翻转整个字符串， 得到的”cde也ab”同lj好就是把原始字符串左旋转2 位的结果。

```java
public String LeftRotateString(String str,int n) {
    if (str == null || str.length() < 2||str.trim().length()<1||n>str.length()) {
        return str;
    }
    char[] chars = str.toCharArray();

    Reverse(chars, 0, n - 1);
    Reverse(chars, n, chars.length - 1);
    Reverse(chars, 0, chars.length - 1);
    return new String(chars);
}

public void Reverse(char[] chs, int start, int end) {
    while (start < end) {
        char temp = chs[start];
        chs[start] = chs[end];
        chs[end] = temp;
        start += 1;
        end -= 1;
    }
}
```



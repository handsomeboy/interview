## 面试题4：替换空格（3.12）


题目：请实现一个函数，把字符串中的每个空格替换成"%20"，例如“We are happy.”，则输出“We%20are%20happy.”。

```java
public class Test04 {
    public static String replaceSpace(StringBuffer str) {
        if (str == null || str.length() < 1) {
            return str.toString();
        }

        StringBuffer newStr = new StringBuffer();

        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == ' ') {
                newStr.append("%20");
            } else {
                newStr.append(str.charAt(i));
            }
        }
        return newStr.toString();
    }

    public static void main(String[] args) {
        StringBuffer str = new StringBuffer("We are happy.");
        System.out.println(replaceSpace(str));
    }
}
```

result:
```
We%20are%20happy.                                               
```





## 面试题35：第一个只出现一次的字符



题目：在字符串中找出第一个只出现一次的字符。

**解法一：直接求解（极度不推荐）**

从头开始扫描这个字符串中的每个字符。当访问到某字符时拿这个字符和后面的每个字符相比较，如果在后面没有发现重复的字符，则该字符就是只出现一次的字符。如果字符串有n个字符，每个字符可能与后面的O(n）个字符相比较，因此这种思路的时间复杂度是O(n^2)。

代码实现：
```java
public int FirstNotRepeatingChar(String str) {
    char[] chars = str.toCharArray();
    for (int i = 0; i < chars.length; i++) {

        if (isOneChar(chars[i],chars)){
            return i;
        }
    }

    return -1;
}

private boolean isOneChar(char ch, char[] chars) {
    int count = 0;
    for (int i = 0; i < chars.length; i++) {

        if (ch == chars[i]) {
            count++;

        }
    }
    return count == 1;
}
```

**第二种：记录法(构建hash)**

由于题目与字符出现的次数相关， 我们是不是可以统计每个字符在该字符串中出现的次数？要达到这个目的，我们需要一个数据容器来存放每个字符的出现次数。在这个数据容器中可以根据字符来查找它出现的次数，也就是说这个容器的作用是把一个字符映射成二个数字。在常用的数据容器中， 哈希表正是这个用途。 

为了解决这个问题，我们可以定义哈希表的键（Key）是字符， 而值(Value ）是该字符出现的次数。同时我们还需要从头开始扫描字符串两次。第一次扫描字符串时，每扫描到一个字符就在哈希表的对应项中把次数加1 。接下来第二次扫描时， 每扫描到一个字符就能从哈希表中得到该字符出现的次数。这样第一个只出现一次的字符就是符合要求的输出。 

第一次扫描时，在哈希表中更新一个字符出现的次数的时间是O(n) 。如果字符串长度为n， 那么第一次扫描的时间复杂度是O(n)。第二次扫描时，同样0(1)能读出一个字符出现的次数，所以时间复杂度仍然是O(n)。这样算起来，总的时间复杂度是O(n)。

实现代码：
```java
public int FirstNotRepeatingChar(String str) {
    HashMap<Character, Integer> map = new HashMap<>();
    for(int i=0;i<str.length();i++) {
        if (map.containsKey(str.charAt(i))) {
            map.put(str.charAt(i), -1);
        }else {
            map.put(str.charAt(i), i);
        }
    }
    int index = Integer.MAX_VALUE;
    for (Map.Entry<Character, Integer> entry : map.entrySet()) {
        if (entry.getValue() >=0  && entry.getValue() < index) {
            index = entry.getValue();
        }
    }
    return index==Integer.MAX_VALUE?-1:index;
}
```

三个扩展题见书上P188.



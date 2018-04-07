## 面试题33：把数组排成最小的数


题目：输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3， 32， 321}，则扫描输出这3 个数字能排成的最小数字321323。


**解法一：直观解法**

先求出这个数组中所有数字的全排列，然后把每个排列拼起来，最后求出拼起来的数字的最大值。

实现代码：
```java
public String PrintMinNumber(int [] numbers) {
    if (numbers == null || numbers.length < 1) {
        return "";
    }
    ArrayList<String> list = new ArrayList<>();
    fullArrray(numbers, 0, numbers.length - 1,list);

    return compare(list);
}

private String compare(ArrayList<String> list) {
                String min = "99999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999";
                for (String s : list) {
                    for (int i=0;i<s.length();i++) {
                        if (min.charAt(i) > s.charAt(i)) {
                            min = s;
                            break;
                        }else if (min.charAt(i) < s.charAt(i)){
                            break;
                        }
        }
    }
    return min;

}

private void fullArrray(int[] numbers, int left, int right, ArrayList<String> list) {
    if (left == right) {
        list.add(toString(numbers));
    }else {
        for (int i=left;i<=right;i++) {
            swap(numbers, left, i);
            fullArrray(numbers, left+1, right, list);
            swap(numbers, left, i);
        }
    }
}

private void swap(int[] numbers, int i, int j) {
    int temp = numbers[i];
    numbers[i] = numbers[j];
    numbers[j] = temp;
}

private String toString(int[] numbers) {
    StringBuilder sb = new StringBuilder();
    for (int number : numbers) {
        sb.append(number);
    }
    return sb.toString();
}
```


根据排列组合的知识，n个数字总共有n!排列，显然不能满足我们的要求。我们再来看一下更快的算法。

**解法二：快排变种（快排算法自己实现，同时使用比较器来进行设计自己的比较算法）**
```java
public class Test331 {
    public String PrintMinNumber(int[] numbers) {
        if (numbers == null || numbers.length < 1) {
            return "";
        }
        MyComparator myComparator = new MyComparator();
        qucksort(numbers, 0, numbers.length - 1, myComparator);

        return toString(numbers);


    }

    private void qucksort(int[] numbers, int left, int right, MyComparator myComparator) {
        if (left < right) {
            int start = left;
            int end = right;
            int base = numbers[left];
            while (left < right) {
                while ((left < right) && myComparator.compare(numbers[right], base) >= 0) {
                    right--;
                }
                numbers[left] = numbers[right];
                while ((left < right) && myComparator.compare(numbers[left], base) <= 0) {
                    left++;
                }
                numbers[right] = numbers[left];
            }

            numbers[left] = base;
            qucksort(numbers, start, left - 1, myComparator);
            qucksort(numbers, left + 1, end, myComparator);
        }


    }


    private String toString(int[] numbers) {
        StringBuilder sb = new StringBuilder();
        for (int number : numbers) {
            sb.append(number);
        }
        return sb.toString();
    }


}

class MyComparator implements Comparator<Integer> {

    @Override
    public int compare(Integer o1, Integer o2) {
        //升序
        String str1 = o1 + "" + o2;
        String str2 = o2 + "" + o1;
        return str1.compareTo(str2);
    }
}
```

**解法二：快排变种（快排算法JDK实现，比较器自己实现）**
```java
public String PrintMinNumber(int[] numbers) {
    if (numbers == null || numbers.length < 1) {
        return "";
    }

    ArrayList<Integer> list = new ArrayList();
    for (int number : numbers) {
        list.add(number);
    }
    Collections.sort(list, new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            //升序
            String str1 = o1 + "" + o2;
            String str2 = o2 + "" + o1;
            return str1.compareTo(str2);
        }
    });

    return toString(list);
}
private String toString(ArrayList<Integer> numbers) {
    StringBuilder sb = new StringBuilder("");
    for (int i=0;i<numbers.size();i++) {
        sb.append(numbers.get(i));
    }
    return sb.toString();
}
```
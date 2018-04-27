## 面试题30：最小的k个数


题目： 输入n个整数，找出其中最小的k个数。例如输入4 、5 、1、6、2、7、3 、8 这8 个数字，则最小的4 个数字是1 、2、3 、4



**解法一：O(n)时间算法，只有可以修改输入数组时可用。**

可以基于Partition函数来解决这个问题。如果基于数组的第k个数字来调整，使得比第k个数字小的所有数字都位于数组的左边，比第k个数字大的所有数字都位于数组的右边。这样调整之后，位于数组中左边的k个数字就是最小的k 个数字（这k 个数字不一定是排序的)。


```java
public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
    ArrayList<Integer> list = new ArrayList<>();
    if (input == null || input.length < 1 || input.length < k || k < 1) {
        return list;
    }
    int left = 0;
    int right = input.length - 1;
    int index = Partition(input, left, right);
    while (index != k - 1) {
        if (index > k - 1) {
            right = index - 1;
            index = Partition(input, left, right);
        } else {
            left = index + 1;
            index = Partition(input, left, right);
        }
    }
    for (int i=0;i<=index;i++) {
        list.add(input[i]);
    }
    return list;
}

//快速排序的Partition函数
private int Partition(int[] array, int left, int right) {
    int base = array[left];
    while (left < right) {
        //从右边开始查找
        while ((left < right) && array[right] > base) {
            right--;
        }
        array[left] = array[right];
        while ((left < right) && array[left] <= base) {
            left++;
        }
        array[right] = array[left];
    }
    array[left] = base;

    return left;
}
```


**解法二： O（nlogk）的算法，特别适合处理海量数据。**

我们可以先创建一个大小为k的数据容器来存储最小的k个数字，接下来我们每次从输入的n个整数中读入一个数。如果容器中已有数字少于k个，则直接把这次读入的整数放入容器中；如果容器中已有k个数字了，也就是容器已满，此时我们不能再插入新的数字了而只能替换已有的数字。找出这已有的k个数中的最大值，然后拿这次待插入的整数和最大值进行比较。如果待插入的值比当前已有的最小值小，则用这个数替换当前已有的最大值；如果待插入的值比当前已有的最大值还大，那么这个数不可能是最小的k个整数之一，于是我们可以抛弃这个整数。

因此当容器满了之后，我们要做3件事；一是在k个整数中找到最大数；二是有可能在这个容器中删除最大数；三是有可能要插入一个新的数字。如果用一个二叉树来实现这个容器，那么我们能在O(logk）时间内实现这三步操作。因此对于n个输入的数字而言，总的时间效率是O(nlogk).

我们可以选择用不同的二叉树来实现这个数据容器。由于每次都需要找到k个整数中的最大数字，我们很容易想到用最大堆。在最大堆中，根节点的值总是大于它的子树中的任意结点的值。于是我们每次可以在O(1）得到已有的k个数字中的最大值，但需要O(logk)时间完成删除及插入操作。

这里我采用PriorityQueue来作为堆（默认是小根堆，要修改成大根堆）

实现代码：
```java
public ArrayList<Integer> GetLeastNumbers_Solution(int[] input, int k) {
    ArrayList<Integer> list = new ArrayList<>();
    if (input == null || input.length < 1 || input.length < k || k < 1) {
        return list;
    }
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(k, new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return o2.compareTo(o1);
        }
    });

    for (int i : input) {
        if (maxHeap.size() != k) {
            maxHeap.offer(i);
        } else {

            if (maxHeap.peek() > i) {
                Integer poll = maxHeap.poll();
                poll = null;
                maxHeap.offer(i);
            }
        }
    }

    for (Integer integer : maxHeap) {
        list.add(integer);
    }
    return list;
}

```


如果对PriorityQueue感兴趣的，可以参考：https://www.cnblogs.com/Elliott-Su-Faith-change-our-life/p/7472265.html
如果对建立堆过程感兴趣的，可以参考：[堆排序](https://www.bcoder.top/2017/08/06/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95%E4%B9%8B%E6%8E%92%E5%BA%8F/#堆排序)




解法比较：
基于函数Partition的第一种解法的平均时间复杂度是O(n)，比第二种思路要快，但同时它也有明显的限制，比如会修改输入的数组。

第二种解法虽然要慢一点，但它有两个明显的优点。一是没有修改输入的数据。二是该算法适合海量数据的输入（包括百度在内的多家公司非常喜欢与海量数据相关的问题）。假如题目是要求从海量的数据中找出最小的k个数字，由于内存的大小是有限的，有可能不能把这些海量数据一次性全部加载入内存。这个时候，我们可以辅助存储空间（比如磁盘）中每次读入一个数字，根据GetLeastNumbers的方式判断是不是需要放入容器LeastNumbers即可。这种思路只要求内存能够容纳leastNumbers即可。因此它适合的情形就是n很大并且k较小的问题。



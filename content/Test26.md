## 面试题26：复杂链表的复制


题目：请实现函数ComplexListNode clone(ComplexListNode head),复制一个复杂链表。在复杂链表中，每个结点除了有一个next 域指向下一个结点外，还有一个sibling 指向链表中的任意结点或者null。

结点结构定义：
```java
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
    RandomListNode() {
        
    }
}

```

思路：首先，下面说明的方法是在不用辅助空间的情况下实现O(n)的时间效率。
第一步：根据原始链表的每个结点N 创建对应的N’。把N’链接在N的后面。
第二步：设置复制出来的结点的sibling。假设原始链表上的N的sibling指向结点S，那么其对应复制出来的N’是N的pext指向的结点，同样S’也是S的next指向的结点。
第三步：把这个长链表拆分成两个链表。把奇数位置的结点用next，链接起来就是原始链表，把偶数位置的结点用next链接起来就是复制出来的链表。

代码实现：
```
public RandomListNode Clone(RandomListNode pHead) {

    //参数检验
    if (pHead == null) {
        return null;
    }

    ////复制next 如原来是A->B->C 变成A->A'->B->B'->C->C'
    RandomListNode pCur = pHead;
    while (pCur != null) {
        RandomListNode node = new RandomListNode(pCur.label);
        node.next = pCur.next;
        pCur.next = node;
        pCur = pCur.next.next;
    }

    //复制random pCur是原来链表的结点 pCur.next是复制pCur的结点
    //临时指针
    pCur = pHead;
    while (pCur != null) {
        if (pCur.random != null){
            pCur.next.random = pCur.random.next;
        }
        pCur = pCur.next.next;
    }

    //拆分链表

    //临时指针
    pCur = pHead;

    RandomListNode  copy = pHead.next;
    //临时指针
    RandomListNode temp = copy;

    while (pCur != null) {
        pCur.next = pCur.next.next;
        if (temp.next != null) {
            temp.next = temp.next.next;
        }
        pCur = pCur.next;
        temp=temp.next;
    }
    return copy;
}
```

做这种题的最好的方式就是画图。另外还有两种一般的解法，详见书上P148.






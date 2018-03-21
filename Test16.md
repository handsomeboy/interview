## 面试题16：反转链表

标签（空格分隔）： ocoder

---
题目：定义一个函数，输入一个链表的头结点，反转该链表并输出反转后链表的头结点。

本题考查基本的编码能力，十分的简单，下面实现两种，一种是非递归，另一种是递归(此种比较难理解)。

非递归方式实现代码：
```java
public ListNode ReverseList(ListNode head) {
    //链表为空或者仅1个数直接返回
    if (head == null || head.next == null) {
        return head;
    }

    ListNode p = head;
    ListNode newHead = null;

    //一直迭代到链尾
    while (p!=null){
        //暂存p下一个地址，防止变化指针指向后找不到后续的数
        ListNode temp = p.next;
        //p->next指向前一个空间
        p.next = newHead;
        //新链表的头移动到p，扩长一步链表
        newHead = p;
        //p指向原始链表p指向的下一个空间
        p = temp;
    }
    return newHead;
}

```


递归方式实现代码：

```
public ListNode ReverseList(ListNode head) {
    //链表为空或者仅1个数直接返回
    if (head == null || head.next == null) {
        return head;
    }
    ListNode newHead = ReverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

要是有不明白的可以参看这篇文章：http://blog.csdn.net/fx677588/article/details/72357389





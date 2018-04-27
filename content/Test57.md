# 面试题57：删除链表中重复的结点

题目：在一个排序的链表中，如何删除重复的结点？




思路：解决这个问题的第一步是确定删除的参数。当然这个函数需要输入待删除链表的头结点。头结点可能与后面的结点重复，也就是说头结点也可能被删除，所以在链表头添加一个结点。 

接下来我们从头遍历整个链表。如果当前结点的值与下一个结点的值相同，那么它们就是重复的结点，都可以被删除。为了保证删除之后的链表仍然是相连的而没有中间断开，我们要把当前的前一个结点和后面值比当前结点的值要大的结点相连。我们要确保prev要始终与下一个没有重复的结点连接在一起。

实现代码：
```java
public ListNode deleteDuplication(ListNode pHead) {
    if (pHead == null || pHead.next == null) {
        return pHead;
    }

    //记录头结点
    ListNode root = new ListNode(0);
    ListNode pre = root;
    ListNode cur = pHead;

    while (cur != null && cur.next != null) {
        if (cur.val == cur.next.val) {
            // 找到下一个不同值的节点，注意其有可能也是重复节点
            while (cur.next != null && cur.next.val == cur.val) {
                cur = cur.next;
            }
            // 指向下一个节点，prev.next也可能是重复结点
            // 所以prev不要移动到下一个结点
            pre.next = cur.next;
        } else {
            // 相邻两个值不同，说明node不可删除，要保留
            pre.next = cur;
            pre = pre.next;


        }

        cur = cur.next;
    }

    return root.next;
}


```
## 面试题17：合并两个排序的链表


题目：输入两个递增排序的链表，合并这两个链表并使新链表中的结点仍然是按照递增排序的。

这题比较简单，也是分为递归和非递归两种方法。

非递归解法（推荐）:
```java
public ListNode Merge(ListNode list1, ListNode list2) {

    //两个链表都为null,直接返回null
    if (list1 == null && list2 == null) {
        return null;
    }

    // 如果第一个链表为空，返回第二个链表头结点
    if (list1 == null) {
        return list2;
    }

    // 如果第二个结点为空，返回第一个链表头结点
    if (list2 == null) {
        return list1;
    }

    //得到新链表的首个结点
    ListNode newHead = null;

    if (list1.val > list2.val) {
        newHead = list2;
        list2 = list2.next;
    }else {
        newHead = list1;
        list1 = list1.next;
    }

    //pointer用于去开拓接下来的节点
    ListNode pointer = newHead;

    while (list1 != null && list2 != null) {
        if (list1.val > list2.val) {
            pointer.next = list2;
            list2 = list2.next;
        }else {
            pointer.next = list1;
            list1 = list1.next;
        }

        // 将指针移动到合并后的链表的末尾
        pointer = pointer.next;
    }


    if (list1!=null){
        pointer.next = list1;
    }
    if (list2 != null) {
        pointer.next = list2;
    }
    return newHead;
}
```

递归的解法（不推荐，递归调用的时候会有方法入栈，需要更多的内存）：
```java
public ListNode Merge(ListNode list1, ListNode list2) {
    // 如果第一个链表为空，返回第二个链表头结点
    if (list1 == null) {
        return list2;
    }

    // 如果第二个链表为空，返回第一个链表头结点
    if (list2 == null) {
        return list1;
    }

    // 记录两个链表中头部较小的结点
    ListNode tmp = list1;
    if (tmp.val < list2.val) {
        // 如果第一个链表的头结点小，就递归处理第一个链表的下一个结点和第二个链表的头结点
        tmp.next = Merge(list1.next, list2);
    } else {
        // 如果第二个链表的头结点小，就递归处理第一个链表的头结点和第二个链表的头结点的下一个结点
        tmp = list2;
        tmp.next = Merge(list1, list2.next);
    }

    // 返回处理结果
    return tmp;
}
```





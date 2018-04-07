## 面试题37：两个链表的第一个公共结点


题目：输入两个链表，找出它们的第一个公共结点。

**解法一：直接法（不推荐）** 

在第一个链表上顺序遍历每个结点，每遍历到一个结点的时候，在第二个链表上顺序遍历每个结点。如果在第二个链表上有一个结点和第一个链表上的结点一样，说明两个链表在这个结点上重合，于是就找到了它们的公共结点。如果第一个链表的长度为m，第二个链表的长度为n，显然该方法的时间复杂度是O(mn） 。

实现代码：
```java
 public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
    if (pHead1 == null || pHead2 == null) {
        return null;
    }

    ListNode temp1 = pHead1;

    while (temp1 != null) {
        ListNode temp2 = pHead2;
        while (temp2 != null) {
            if (temp1.val == temp2.val) {
                return temp2;
            }
            temp2 = temp2.next;
        }
        temp1 = temp1.next;
    }
    return null;
}
```




**解法二：使用栈**

从链表结点的定义可以看出，这两个链表是单链表。如果两个单向链表有公共的结点，那么这两个链表从某一结点开始，它们的m_pNext都指向同一个结点，但由于是单向链表的结点，之后他们所有结点都是重合的，不可能再出现分叉。所以两个有公共结点而部分重合的链表，拓扑形状开起来像一个Y，而不可能项X。

经过分析我们发现，如果两个链表有公共结点，那么公共结点出现在两个链表的尾部。如果我们从两个链表的尾部开始往前比较，最后一个相同的结点就是我们要找的结点。可问题是在单向链表中，我们只能从头结点开始按顺序遍历，最后才能到达尾节点。最后到达的尾节点却要最先被比较，这听起来是不是像后进先出，也是我们就能想到用栈的特点来解决这个问题：分别把两个链表的结点放入两个栈里，这样两个链表的尾节点就位于两个栈的栈顶，接下来比较两个栈顶的结点是否相同。如果相同，则把栈顶弹出接着比较下一个栈顶，直到找到最后一个相同的结点。

在上述思路中，我们需要用两个辅助栈。如果链表的长度分别为m和n，那么空间复杂度是O(m+n)。这种思路的时间复杂度也是O(m+n）.和最开始的蛮力法相比，时间效率得到了提高，相当于是用空间消耗换取了时间效率。


```java
public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
    if (pHead1 == null || pHead2 == null) {
        return null;
    }

    ListNode temp1 = pHead1;
    ListNode temp2 = pHead2;
    Stack<ListNode> s1 = new Stack<>();
    Stack<ListNode> s2 = new Stack<>();
    while (temp1 != null) {
        ListNode temp = temp1;
        s1.push(temp);
        temp1 = temp1.next;
    }
    while (temp2 != null) {
        ListNode temp = temp2;
        s2.push(temp);
        temp2 = temp2.next;
    }

    ListNode result = null;
    while ((!s1.isEmpty())&&(!s2.isEmpty())){
        if (s1.peek().val != s2.peek().val) {
            break;
        }else{
            result=s1.pop();
            s2.pop();
        }

    }
    return result;

}
```


**解法三：先行法（推荐）** 
首先遍历两个链表得到他们的长度，就能知道哪个链表比较长，以及长的链表比短的链表多几个结点。在第二次遍历的时候，在较长的链表上先走若干步，接着再同时在两个链表上遍历，找到第一个相同的结点就是他们的第一个公共结点。

实现代码：
```java
public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
    if (pHead1 == null || pHead2 == null) {
        return null;
    }

    int pHead1Length = NodeLength(pHead1);
    int pHead2Length = NodeLength(pHead2);

    ListNode l = pHead1;//long
    ListNode s = pHead2;//short
    int diff = pHead1Length - pHead2Length;
    if (diff < 0) {
        l = pHead2;
        s = pHead1;
        diff = -diff;
    }
    for (int i=0;i<diff;i++) {
        l = l.next;
    }
    while ((l != null) && (s != null) && (s.val != l.val)) {
        l = l.next;
        s = s.next;
    }
    return l;
}

private int NodeLength(ListNode pHead1) {
    int count = 0;
    ListNode temp = pHead1;
    while (pHead1 != null) {
        count++;
        pHead1 = pHead1.next;
    }
    return count;
}
```
第三种思路比第二种思路相比，时间复杂度为O(m+n)，但我们不在需要辅助栈，因此提高了空间效率。
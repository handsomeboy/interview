## 面试题15：链表中倒数第k个结点



题目：输入一个链表，输出该链表中倒数第k 个结点．为了符合大多数人的习惯，本题从1 开始计数，即链表的尾结点是倒数第1 个结点．例如一个链表有6 个结点，从头结点开始它们的值依次是1 、2、3、4、5 、6。这个个链表的倒数第3 个结点是值为4 的结点．

为了得到第K个结点，很自然的想法是先走到链表的尾端，再从尾端回溯K步。可是我们从链表结点的定义可疑看出本题中的链表 是单向链表，单向链表的结点只有从前往后的指针而没有从后往前的指针，因此这种思路行不通。

既然不能从尾节点开始遍历这个链表，我们还是把思路回到头结点上来。假设整个链表有N个结点，那么倒数第K哥结点就是从头结点开始的第n-k-1个结点。如果我们只要从头结点开始往后走n-k+1步就可疑了。如何得到节点数n?这个不难，只需要从头开始遍历链表，没经过一个结点，计数器加1就行了。

这种方式实现代码很简单：
实现代码如下：
```java
public ListNode FindKthToTail(ListNode head,int k) {
    if (head ==null||k>0){
        return null;
    }

    int count = 0;
    ListNode temp = head;
    while (temp!=null){
        count++;
        temp = temp.next;
    }

    if (count <k){
        return null;
    }

    int skip = count-k+1;
    while (skip!=1) {
        head = head.next;
        skip--;
    }
    return head;
}
```

测试代码如下：
```
@Test
public void test() {
    ListNode head = new ListNode();
    head.val = 1;

    head.next = new ListNode();
    head.next.val = 2;

    head.next.next = new ListNode();
    head.next.next.val = 3;

    head.next.next.next = new ListNode();
    head.next.next.next.val = 4;

    head.next.next.next.next = new ListNode();
    head.next.next.next.next.val = 5;

    head.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.val = 6;

    head.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.val = 7;

    head.next.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.next.val = 8;

    head.next.next.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.next.next.val = 9;

    System.out.println(FindKthToTail(head, 1).val); // 倒数第一个
    System.out.println(FindKthToTail(head, 5).val); // 中间的一个
    System.out.println(FindKthToTail(head, 9).val); // 倒数最后一个就是顺数第一个

    System.out.println(FindKthToTail(head, 10));
}
```


result:
```
9
5
1
null
```


这种方式能满足我们的需求，时间复杂度为O(n),但是我们需要遍历链表两次，第一次统计出链表中结点的个数，第二次就能找到倒数第k个结点。

当然，这不是最好的解法，更好的解法是遍历链表一次。

具体思路如下：
为了实现只遍历链表一次就能找到倒数第k个结点，我们可以定义两个指针。第一个指针从链表的头指针开始遍历向前走k-1。第二个指针保持不动；从第k步开始，第二个指针也开化寺从链表的头指针开始遍历。由于两个指针的距离保持在k-1，当第一个（走在前面的）指针到达链表的尾指结点时，第二个指针正好是倒数第k个结点。

实现代码：
```java
public ListNode FindKthToTail(ListNode head,int k) {
    // 输入的链表不能为空，并且k大于0
    if (head ==null||k==0){
        return null;
    }

    ListNode tempNode = head;
    // 倒数第k个结点与倒数第一个结点相隔k-1个位置
    while (k!=1){
        if (tempNode.next==null){
            return null;
        }
        tempNode = tempNode.next;
        --k;
    }
    ListNode result = head;
    while (tempNode.next!=null){
        tempNode = tempNode.next;
        result = result.next;
    }
    return result;

}
```








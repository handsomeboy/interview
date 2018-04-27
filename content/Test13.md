## 面试题13：在O(1)时间删除链表节点


题目：给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间删除该节点。

在实现代码之前定义链表的结构：
```java
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
    ListNode() {
    }
}
```

在单向链表中删除一个节点，最常规的方法无疑是从链表的头结点开始，顺序遍历查找要删除的节点，并在链表中删除该节点。

之所以需要从开头开始查找，是需要得到被删除节点的前一个节点h。但是查找h节点并不是必须的，我们可以很方便地得到要删除节点的下一节点j，把j节点的内容复制到i，然后删除j节点(图见书P99)

    
实现代码：
```java
/**
 * @param head        头节点
 * @param toBeDeleted 待删除的节点
 * @return
 */
public ListNode deleteNode(ListNode head, ListNode toBeDeleted) {

    if (head == null || toBeDeleted == null) {
        return head;
    }

    //待删节点是头节点
    if (head == toBeDeleted) {

        return head.next;
    }

    //待删节点是尾节点
    if (toBeDeleted.next == null) {
        ListNode temp = head;
        while (temp.next != toBeDeleted) {
            temp = temp.next;
        }
        temp.next = null;
        return head;
    }

    //待删节点是中间节点
    // 将下一个结点的值输入当前待删除的结点
    toBeDeleted.val = toBeDeleted.next.val;
    // 待删除的结点的下一个指向原先待删除引号的下下个结点，即将待删除的下一个结点删除
    toBeDeleted.next = toBeDeleted.next.next;

    return head;
}
```

测试代码：
```java
/**
 * 输出链表的元素值
 *
 * @param head 链表的头结点
 */
public void printList(ListNode head) {
    while (head != null) {
        System.out.print(head.val + "->");
        head = head.next;
    }
    System.out.println("null");
}

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

    ListNode middle = head.next.next.next.next = new ListNode();
    head.next.next.next.next.val = 5;

    head.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.val = 6;

    head.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.val = 7;

    head.next.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.next.val = 8;

    ListNode last = head.next.next.next.next.next.next.next.next = new ListNode();
    head.next.next.next.next.next.next.next.next.val = 9;

    head = deleteNode(head, null); // 删除的结点为空
    printList(head);
    ListNode node = new ListNode();
    node.val = 12;

    head = deleteNode(head, head); // 删除头结点
    printList(head);
    head = deleteNode(head, last); // 删除尾结点
    printList(head);
    head = deleteNode(head, middle); // 删除中间结点
    printList(head);

    head = deleteNode(head, node); // 删除的结点不在链表中
    printList(head);
}
```


result:
```
1->2->3->4->5->6->7->8->9->null
2->3->4->5->6->7->8->9->null
2->3->4->5->6->7->8->null
2->3->4->6->7->8->null

java.lang.NullPointerException
```
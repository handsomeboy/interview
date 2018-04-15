# 面试题56：链表中环的入口结点
题目：一个链表中包含环，如何找出环的入口结点？

思路：可以用两个指针来解决这个问题。先定义两个指针P1和P2指向链表的头结点。如果链表中环有n个结点，指针P1在链表上向前移动n步，然后两个指针以相同的速度向前移动。当第二个指针指向环的入口结点时，第一个指针已经围绕着环走了一圈又回到了入口结点。 

剩下的问题就是如何得到环中结点的数目。我们在面试题15的第二个相关题目时用到了一快一慢的两个指针。如果两个指针相遇，表明链表中存在环。两个指针相遇的结点一定是在环中。可以从这个结点出发，一边继续向前移动一边计数，当再次回到这个结点时就可以得到环中结点数了。

下面给出证明：
假设快慢指针相遇，慢指针走了x的路程，则快指针走了y=2x的路程，我们知道快指针比慢指针多走了一圈，所以圈长c=y-x=x

再假设慢指针走到入口结点处是m,走了圈中的距离是n,那么x=m+n，由于圈长是x,而慢指针在n处，因此慢指针再走m步就到了入口处，而m恰恰是慢指针走到入口结点处的路程，因此，再来一个步长为1的从根节点走，那么相遇就是入口结点。

实现代码：
```java
public ListNode EntryNodeOfLoop(ListNode pHead) {
    if (pHead == null || pHead.next == null) {
        return null;
    }

    ListNode fast = pHead;
    ListNode slow = pHead;
    while (fast != null && fast.next != null) {
        fast = fast.next.next;
        slow = slow.next;
        if (fast == slow) {
            break;
        }
    }

    if (fast == null || fast.next == null) {
        return null;
    }
    fast = pHead;
    while (fast != slow) {
        fast = fast.next;
        slow = slow.next;
    }
    return fast;

}
```
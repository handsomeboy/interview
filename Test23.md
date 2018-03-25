## 面试题23：从上往下打印二叉树



题目：从上往下打印出二叉树的每个结点，同一层的结点按照从左向右的顺序打印。

思路：这道题实质是考查树的遍历算法。从上到下打印二叉树的规律：每一次打印一个结点的时候，如果该结点有子结点， 则把该结点的子结点放到一个队列的末尾。接下来到队列的头部取出最早进入队列的结点，重复前面的打印操作，直至队列中所有的结点都被打印出来为止。

实现代码：
```java
public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
    ArrayList<Integer> list = new ArrayList<>();
    if (root == null) {
        return list;
    }
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode pop = queue.remove();
        list.add(pop.val);
        if (pop.left != null) {
            queue.add(pop.left);
        }
        if (pop.right != null) {
            queue.add(pop.right);
        }
    }
    return list;
}
```
测试代码：
```java
private void printFromToBottom(TreeNode root) {
    if (root == null) {
        return;
    }
    for (Integer integer : PrintFromTopToBottom(root)) {
        System.out.print(integer+ " ");
    }
}

@Test
public void test() {
    //       8
    //    /    \
    //   6     10
    //  / \   / \
    // 5   7 9  11
    TreeNode root = new TreeNode();
    root.val = 8;
    root.left = new TreeNode();
    root.left.val = 6;
    root.left.left = new TreeNode();
    root.left.left.val = 5;
    root.left.right = new TreeNode();
    root.left.right.val = 7;
    root.right = new TreeNode();
    root.right.val = 10;
    root.right.left = new TreeNode();
    root.right.left.val = 9;
    root.right.right = new TreeNode();
    root.right.right.val = 11;
    printFromToBottom(root);

    //         1
    //        /
    //       3
    //      /
    //     5
    //    /
    //   7
    //  /
    // 9
    TreeNode root2 = new TreeNode();
    root2.val = 1;
    root2.left = new TreeNode();
    root2.left.val = 3;
    root2.left.left = new TreeNode();
    root2.left.left.val = 5;
    root2.left.left.left = new TreeNode();
    root2.left.left.left.val = 7;
    root2.left.left.left.left = new TreeNode();
    root2.left.left.left.left.val = 9;
    System.out.println("\n");
    printFromToBottom(root2);

    // 0
    //  \
    //   2
    //    \
    //     4
    //      \
    //       6
    //        \
    //         8
    TreeNode root3 = new TreeNode();
    root3.val = 0;
    root3.right = new TreeNode();
    root3.right.val = 2;
    root3.right.right = new TreeNode();
    root3.right.right.val = 4;
    root3.right.right.right = new TreeNode();
    root3.right.right.right.val = 6;
    root3.right.right.right.right = new TreeNode();
    root3.right.right.right.right.val = 8;
    System.out.println("\n");
    printFromToBottom(root3);

    // 1
    TreeNode root4 = new TreeNode();
    root4.val = 1;
    System.out.println("\n");
    printFromToBottom(root4);

    // null
    System.out.println("\n");
    printFromToBottom(null);


}
```

result:
```
8 6 10 5 7 9 11 

1 3 5 7 9 

0 2 4 6 8 

1 
```

做这题的时候竟然把Queue接口的实现类给忘记了，尴尬，有时间抓紧去复习一下。


题目扩展：
如何广度优先遍历一个有向图？这同样也可以基于队列实现。树是图的一种特殊退化形式，从上到下按层遍历二叉树，从本质上来说就是广度优先遍历二叉树。


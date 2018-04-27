# 面试题61：按之字形顺序打印二叉树

题目：请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，即第一行按照从左到右的顺序打印，第二层按照从右到左顺序打印，第三行再按照从左到右的顺序打印，其他以此类推。

代码实现：
```java
public ArrayList<ArrayList<Integer>> Print(TreeNode pRoot) {
    ArrayList<ArrayList<Integer>> lists = new ArrayList<>();
    if (pRoot == null) {
        return lists;
    }

    int current = 1;
    int next = 0;
    LinkedList<TreeNode> list = new LinkedList<>();
    TreeNode node;
    ArrayList<Integer> temp = new ArrayList<>();
    list.add(pRoot);

    while (list.size() > 0) {
        node = list.remove(0);
        current--;
        temp.add(node.val);

        if (node.left != null) {
            list.add(node.left);
            next++;
        }
        if (node.right != null) {
            list.add(node.right);
            next++;
        }

        if (current ==0) {
            lists.add(temp);
            temp = new ArrayList<>();
            current = next;
            next = 0;
        }
    }

    return lists;
}
```




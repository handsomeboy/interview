# 面试题60：把二叉树打印出多行
题目：从上到下按层打印二叉树，同一层的结点按从左到右的顺序打印，每一层打印一行。

思路:用一个队列来保存将要打印的结点。为了把二叉树的每一行单独打印到一行里，我们需要两个变量：一个变量表示在当前的层中还没有打印的结点数，另一个变量表示下一次结点的数目。

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







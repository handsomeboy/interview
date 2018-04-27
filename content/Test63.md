# 面试题63：二叉搜索树的第k个结点

题目：给定一棵二叉搜索树，请找出其中的第k大的结点。

思路:如果按照中序遍历的顺序遍历一棵二叉搜索树，遍历序列的数值是递增排序的。只需要用中序遍历算法遍历一棵二叉搜索树，就很容易找出它的第k大结点。

实现代码：
```java
TreeNode KthNode(TreeNode pRoot, int k) {
    if (pRoot == null||k<1) {
        return null;
    }

    ArrayList<TreeNode> list = new ArrayList<>();
    order(pRoot, list);
    if (list.size() < k) {
        return null;
    }
    return list.get(k - 1);
}

private void order(TreeNode pRoot, ArrayList<TreeNode> list) {
    if (pRoot == null) {
        return;
    }
    order(pRoot.left, list);
    list.add(pRoot);
    order(pRoot.right, list);

}

```





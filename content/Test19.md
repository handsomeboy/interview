## 面试题19：二叉树的镜像



题目：请完成一个函数，输入一个二叉树，该函数输出它的镜像。



二叉树的镜像定义：
源二叉树 
```
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
```
镜像二叉树:
```
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

思路：
我们先前序遍历这棵树的每个结点，如果遍历到的结点有子结点，就交换它的两个子结点。当交换完所有非叶子结点的左右子结点之后，就得到了树的镜像.


递归实现代码：
```java
public void Mirror(TreeNode root) {
    if (root != null) {
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        Mirror(root.left);
        Mirror(root.right);
    }
}
```
非递归实现代码：
```java
public void Mirror(TreeNode root) {

    TreeNode temp = root;
    Stack<TreeNode> stack = new Stack<>();
    while (!stack.isEmpty() || temp != null) {
        while (temp != null) {
            TreeNode t = temp.left;
            temp.left = temp.right;
            temp.right = t;
            stack.push(temp);
            temp = temp.left;
        }
        temp = stack.pop();
        temp = temp.right;
    }
}
```

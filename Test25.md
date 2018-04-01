## 面试题25：二叉树中和为某一值的路径

题目：输入一棵二叉树和一个整数， 打印出二叉树中结点值的和为输入整数的所有路径。从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。


思路：
由于路径是从根结点出发到叶结点，也就是说路径总是以根结点为起始点，因此我们首先需要遍历根结点。在树的前序、中序、后序三种遍历方式中，只有前序遍历是首先访问根结点的。

当用前序遍历的方式访问到某一结点时，我们把该结点添加到路径上，并累加该结点的值。如果该结点为叶结点并且路径中结点值的和刚好等于输入的整数，则当前的路径符合要求，我们把它打印出来。如果当前结点不是叶结点，则继续访问它的子结点。当前结点访问结束后，递归函数将自动回到它的父结点。因此我们在函数退出之前要在路径上删除当前结点并减去当前结点的值，以确保返回父结点时路径刚好是从根结点到父结点的路径。

不难看出保存路径的数据结构实际上是一个枝， 因为路径要与递归调用状态一致， 而递归调用的本质就是一个压栈和出栈的过程。

实现代码：
```java
ArrayList<ArrayList<Integer>> lists = new ArrayList<>();
ArrayList<Integer> list = new ArrayList<Integer>();

public ArrayList<ArrayList<Integer>> FindPath(TreeNode root, int target) {

    //验证输入条件
    if (root == null) {
        return lists;
    }

    FindPath(root, 0, target);
    return lists;


}

private void FindPath(TreeNode root, int curVal, int target) {
    if (root != null) {
        // 加上当前结点的值
        curVal += root.val;
        // 将当前结点入队
        list.add(root.val);

        // 如果当前结点的值小于期望的和
        if (curVal < target) {

            // 递归处理左子树
            FindPath(root.left, curVal, target);

            // 递归处理右子树
            FindPath(root.right, curVal, target);

            // 如果当前和与期望的和相等,并且是叶子节点
        } else if (curVal == target && root.right == null && root.left == null) {

            //注意这里添加数据的方式，是new 不是直接将list加进去
            lists.add(new ArrayList<>(list));
        }

        // 移除当前结点（出栈）
        list.remove(list.size() - 1);
    }
}

```

测试代码：
```java
@Test
public void test() {
    //            10
    //         /      \
    //        5        12
    //       /\
    //      4  7
    TreeNode root = new TreeNode();
    root.val = 10;
    root.left = new TreeNode();
    root.left.val = 5;
    root.left.left = new TreeNode();
    root.left.left.val = 4;
    root.left.right = new TreeNode();
    root.left.right.val = 7;
    root.right = new TreeNode();
    root.right.val = 12;

    // 有两条路径上的结点和为22
    System.out.println("FindPath(root, 22)：" + new Test25().FindPath(root, 22));

    // 没有路径上的结点和为15
    System.out.println("FindPath(root, 15):"+new Test25().FindPath(root, 15));
    FindPath(root, 15);

    // 有一条路径上的结点和为19
    System.out.println("FindPath(root, 19):"+new Test25().FindPath(root, 19));
    FindPath(root, 19);


    //               5
    //              /
    //             4
    //            /
    //           3
    //          /
    //         2
    //        /
    //       1
    TreeNode root2 = new TreeNode();
    root2.val = 5;
    root2.left = new TreeNode();
    root2.left.val = 4;
    root2.left.left = new TreeNode();
    root2.left.left.val = 3;
    root2.left.left.left = new TreeNode();
    root2.left.left.left.val = 2;
    root2.left.left.left.left = new TreeNode();
    root2.left.left.left.left.val = 1;

    // 有一条路径上面的结点和为15
    System.out.println("FindPath(root2, 15):"+new Test25().FindPath(root2, 15));

    // 没有路径上面的结点和为16
    System.out.println("FindPath(root2, 16):"+new Test25().FindPath(root2, 16));

    // 1
    //  \
    //   2
    //    \
    //     3
    //      \
    //       4
    //        \
    //         5
    TreeNode root3 = new TreeNode();
    root3.val = 1;
    root3.right = new TreeNode();
    root3.right.val = 2;
    root3.right.right = new TreeNode();
    root3.right.right.val = 3;
    root3.right.right.right = new TreeNode();
    root3.right.right.right.val = 4;
    root3.right.right.right.right = new TreeNode();
    root3.right.right.right.right.val = 5;

    // 有一条路径上面的结点和为15
    System.out.println("FindPath(root3, 15):"+new Test25().FindPath(root3, 15));

    // 没有路径上面的结点和为16
    System.out.println("FindPath(root3, 16):"+new Test25().FindPath(root3, 16));

    // 树中只有1个结点
    TreeNode root4 = new TreeNode();

    root4.val = 1;
    // 有一条路径上面的结点和为1
    System.out.println("FindPath(root4, 1):"+new Test25().FindPath(root4, 1));

    // 没有路径上面的结点和为2
    System.out.println("FindPath(root4, 2):"+new Test25().FindPath(root4, 2));

    // 树中没有结点
    System.out.println("FindPath(null, 0):"+new Test25().FindPath(null, 0));
}
```

result:
```
FindPath(root, 22)：[[10, 5, 7], [10, 12]]
FindPath(root, 15):[]
FindPath(root, 19):[[10, 5, 4]]
FindPath(root2, 15):[[5, 4, 3, 2, 1]]
FindPath(root2, 16):[]
FindPath(root3, 15):[[1, 2, 3, 4, 5]]
FindPath(root3, 16):[]
FindPath(root4, 1):[[1]]
FindPath(root4, 2):[]
FindPath(null, 0):[]
```
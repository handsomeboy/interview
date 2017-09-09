# 什么是二叉查找树？如何用java实现?


## 什么是二叉查找树?

二叉查找树(Binary Search Tree)，又被称为二叉搜索树。
它是特殊的二叉树：对于二叉树，假设x为二叉树中的任意一个结点，x节点包含关键字key，节点x的key值记为key[x]。如果y是x的左子树中的一个结点，则key[y] <= key[x]；如果y是x的右子树的一个结点，则key[y] >= key[x]。那么，这棵树就是二叉查找树。如下图所示：

![什么是二叉查找树？如何用java实现?](http://www.bcoder.top/img/interview/37.jpg)



在二叉查找树中：
(1) 若任意节点的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
(2) 任意节点的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
(3) 任意节点的左、右子树也分别为二叉查找树。
(4) 没有键值相等的节点（no duplicate nodes）

## 二叉查找树的java实现

### 节点定义
```java
public class BSTree<T extends Comparable<T>> {

    private BSTNode<T> mRoot;    // 根结点

    public class BSTNode<T extends Comparable<T>> {
        T key;                // 关键字(键值)
        BSTNode<T> left;      // 左孩子
        BSTNode<T> right;     // 右孩子
        BSTNode<T> parent;    // 父结点

        public BSTNode(T key, BSTNode<T> parent, BSTNode<T> left, BSTNode<T> right) {
            this.key = key;
            this.parent = parent;
            this.left = left;
            this.right = right;
        }
    }

        ......
}
```

BSTree是二叉树，它保护了二叉树的根节点mRoot；mRoot是BSTNode类型，而BSTNode是二叉查找树的节点，它是BSTree的内部类。BSTNode包含二叉查找树的几个基本信息：
(01) key -- 它是关键字，是用来对二叉查找树的节点进行排序的。
(02) left -- 它指向当前节点的左孩子。
(03) right -- 它指向当前节点的右孩子。
(04) parent -- 它指向当前节点的父结点。

## JAVA代码实现


```

public class BinarySearchTree<T extends Comparable<T>>{

    private static class Node<T>{
        private T data;
        private Node<T> left;
        private Node<T> right;
        
        public Node(T data){
            this(data,null,null);
        }
        public Node(T data, Node<T> left, Node<T> right) {
            this.data = data;
            this.left = left;
            this.right = right;
        }
    }

    
    //私有变量 根节点root
    private Node<T> root;
    
    
    //无参构造函数,根节点为null
    public BinarySearchTree(){
        root=null;
    }
    
    
    //清空二叉查找树
    public void makeEmpty(){
        root=null;
    }
    
    
    //判断树是否为空
    public boolean isEmpty(){
        
        return root==null;
    }
    
    
    //查找集合中是否有元素value,有返回true
    public boolean contains(T value){
        return contains(value,root);
    }
    
    
    private boolean contains(T value, Node<T> t) {
        if(t==null){
            return false;
        }
        int result=value.compareTo(t.data);
        if(result<0){
            return contains(value,t.left);
        }else if(result>0){
            return contains(value,t.right);
        }else{
            return true;
        }
    }


    //查找集合中的最小值
    public T findMin(){
        return  findMin(root).data;
    }
    
    
    private Node<T> findMin(Node<T> t) {
        if(t==null){
            return null;
        }else if(t.left==null){
            return t;
        }
        return findMin(t.left);
    }
    

    //查找集合中的最大值
    public T findMax(){
        return findMax(root).data;
    }
    
    
    
    
    private Node<T> findMax(Node<T> t) {
        if(t!=null){
            while(t.right!=null){
                t=t.right;
            }
        }
        
        return t;
    }
    
    

    //插入元素
    public void insert(T value){
        
        root =insert(value,root);
    }



    private Node<T> insert(T value, Node<T> t) {
        if(t==null){
            return new Node(value,null,null);
        }
        int result=value.compareTo(t.data);
        if(result<0){
            t.left=insert(value,t.left);
        }else if(result>0){
            t.right=insert(value,t.right);
        }
        return t;
    }
    
    
    
    //移除元素
    public void remove(T value){
        root=remove(value,root);
        
        
    }



    private Node<T> remove(T value, Node<T> t) {
        if(t==null){
            return t;
        }
        
        int result=value.compareTo(t.data);
        if(result<0){
            t.left=remove(value,t.left);
        }else if(result>0){
            t.right=remove(value,t.right);
        }else if(t.left!=null&&t.right!=null){//如果被删除节点有两个儿子
            //1.当前节点值被其右子树的最小值代替
            t.data=findMin(t.right).data;
            //将右子树的最小值删除
            t.right=remove(t.data, t.right);
        }else{
            //如果被删除节点是一个叶子 或只有一个儿子
            t=(t.left!=null)?t.left:t.right;
        }
        
        return t;
    }
    
    
    
    //中序遍历打印
    public void printTree(){
        printTree(root);
    }

    private void printTree(Node<T> t) {
        
        if(t!=null){
            printTree(t.left);
            System.out.println(t.data);
            printTree(t.right);
        }
    }
    
}
```


代码：
```java

public class TestBST {

    public static void main(String[] args) {
        
        BinarySearchTree<Integer> bst=new BinarySearchTree<>();
        bst.insert(5);
        bst.insert(7);
        bst.insert(3);
        bst.insert(1);
        bst.insert(9);
        bst.insert(6);
        bst.insert(4);
        System.out.println("最小值:"+bst.findMin());
        System.out.println("最大值:"+bst.findMax());
        System.out.println("查找元素9是否存在:"+bst.contains(9));
        System.out.println("查找元素8是否存在:"+bst.contains(8));
        System.out.println("中序遍历二叉树");
        bst.printTree();
    }
}
```

运行结果：

```
最小值:1
最大值:9
查找元素9是否存在:true
查找元素8是否存在:false
中序遍历二叉树
1
3
4
5
6
7
9
```

## 二叉搜索树的效率

树的大部分操作需要从上至下一层层的查找树的节点，对于一棵满树，大约有一半的节点处于最底层（最底层节点数 = 其它层节点数的和 + 1），故节点操作大约有一半需要找到最底层节点，大约有四分之一的节点处于倒数第二层，故节点操作大约有四分之一需要找到倒数第二层节点，依此类推

查找过程中，需要访问每一层的节点，故只要知道了查找的层数，就能知道操作所需的时间，如果节点总数为N，层数为L，`L=log2(N+1)`

如果为查找操作或删除操作，被操作的节点可能是是树的任意节点，故查找操作或删除操作的时间复杂度为：1/21*log2(N+1) + 1/22*log2(N/2+1) + ... + 1/2N*1

如果为插入操作，由于每次都在树的最低层插入新的节点，故插入操作的时间复杂度为：`log2(N+1)`

总的来说可以认为二叉搜索树操作的时间复杂度为为`O(logN)`

如果树不是一棵满树，则判断起来比较复杂，但是如果层数相同，对满树的操作肯定比对不满树的操作更耗时


对于一个含有10000个数据项的有序链表，查找操作平均需要比较5000次，对于一个含有10000个节点的二叉搜索树，查找操作大约需要13次

对于一个含有10000个数据项的有序数组，插入操作平均需要移动5000次（对于比较次数，使用不同的算法比较次数并不相同），对于一个含有10000个节点的二叉搜索树，插入操作只需大约13次比较就可找到待插入节点的插入位置，并且由于该位置总是处于二叉搜索树的最底层，并不需要移动其它的节点

可以看出，二叉搜索树集合了有序链表插入删除效率高和有序数组查询效率高的优点。





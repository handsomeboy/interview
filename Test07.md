## 面试题7：用两个栈实现队列


题目：用两个栈实现一个队列。队列的声明如下，请实现它的两个函数appendTail 和deleteHead，分别完成在队列尾部插入结点和在队列头部删除结点的功能。

实现代码：
```java
public class Test07 {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        stack1.push(node);
    }

    public int pop() {

        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()){
                stack2.add(stack1.pop());
            }
        }

        if (stack2.isEmpty()) {
            throw new RuntimeException("stack is empty!!");
        }

        return stack2.pop();
    }
}
```






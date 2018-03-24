## 面试题21：包含min函数的栈


题目： 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小素的min 函数。在该栈中，调用min、push 及pop的时间复杂度都是0(1)


思路：
把每次的最小元素（之前的最小元素和新压入战的元素两者的较小值）都保存起来放到另外一个辅助栈里.

如果每次都把最小元素压入辅助栈，那么就能保证辅助栈的栈顶一直都是最小元素．当最小元素从数据栈内被弹出之后，同时弹出辅助栈的栈顶元素，此时辅助栈的新栈顶元素就是下一个最小值。

代码实现：
```java
public class Test21 {
    Stack<Integer> stack;
    Stack<Integer> minStarck;

    public Test21() {
        stack = new Stack<>();
        minStarck = new Stack<>();
    }

    public void push(int node) {
        stack.push(node);
        if (minStarck.isEmpty()) {
            minStarck.push(node);
        } else if (minStarck.peek() >= node) {
            minStarck.push(node);
        } else {
            minStarck.push(minStarck.peek());
        }
    }

    public void pop() {
        if (stack.isEmpty()) {
            throw new RuntimeException("stack is empty");
        } else {
            stack.pop();
            minStarck.pop();
        }
    }

    public int top() {
        if (stack.isEmpty()) {
            throw new RuntimeException("stack is empty");
        } else {
            return stack.peek();
        }
    }

    public int min() {
        if (stack.isEmpty()) {
            throw new RuntimeException("stack is empty");
        } else {
            return minStarck.peek();
        }
    }
}
```








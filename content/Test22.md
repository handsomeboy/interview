## 面试题22：栈的压入、弹出序列



题目：输入两个整数序列，第一个序列表示栈的压入顺序，请判断二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1，2，3，4，5是某栈的压栈序列，序列，4，5，3，2，1是该压栈序列对应的一个弹出序列，但4，3，5，1，2就不可能是该压栈序列的弹出序列。

思路：解决这个问题很直观的想法就是建立一个辅助栈，把输入的第一个序列中的数字依次压入该辅助栈，并按照第二个序列的顺序依次从该栈中弹出数字。

判断一个序列是不是栈的弹出序列的规律：如果下一个弹出的数字刚好是栈顶数字，那么直接弹出。如果下一个弹出的数字不在栈顶，我们把压栈序列中还没有入栈的数字压入辅助栈，直到把下一个需要弹出的数字压入栈顶为止。如果所有的数字都压入栈了仍然没有找到下一个弹出的数字，那么该序列不可能是一个弹出序列。


代码实现：
```java
public boolean IsPopOrder(int [] pushA,int [] popA) {
    //检查输入
    if (pushA == null || popA == null||pushA.length!=popA.length) {
        return false;
    }

    boolean result = true;//操作结果
    Stack<Integer> stack = new Stack<>();
    int index = 0;//即将压入栈中的数字

    for (int i=0;i<popA.length;i++) {

        //当前数为pop序列的数，直接拿出，无需入栈
        if (index<popA.length&&popA[i] == pushA[index]){
            index++;
            continue;
        }
        if (!stack.isEmpty() && stack.peek() == popA[i]) {
            stack.pop();
            continue;
        }
        boolean temp = false;
        while (index<popA.length){
            if (pushA[index] ==popA[i]){
                index++;
                temp = true;
                break;
            }
            stack.push(pushA[index++]);
        }
        if (!temp){
            result = false;
            break;
        }
    }
    return result;
}
```

测试代码：
```java
@Test
public void test() {
    int[] push = {1, 2, 3, 4, 5};
    int[] pop1 = {4, 5, 3, 2, 1};
    int[] pop2 = {3, 5, 4, 2, 1};
    int[] pop3 = {4, 3, 5, 1, 2};
    int[] pop4 = {3, 5, 4, 1, 2};

    System.out.println("true: " + IsPopOrder(push, pop1));
   System.out.println("true: " + IsPopOrder(push, pop2));
    System.out.println("false: " + IsPopOrder(push, pop3));
    System.out.println("false: " + IsPopOrder(push, pop4));

    int[] push5 = {1};
    int[] pop5 = {2};
    System.out.println("false: " + IsPopOrder(push5, pop5));

    int[] push6 = {1};
    int[] pop6 = {1};
    System.out.println("true: " + IsPopOrder(push6, pop6));

    System.out.println("false: " + IsPopOrder(null, null));
}
```

result:
```java
true: true
true: true
false: false
false: false
false: false
true: true
false: false
```


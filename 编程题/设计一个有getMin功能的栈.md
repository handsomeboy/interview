# 设计一个有getMin功能的栈



### 【题目】


实现一个特殊的栈，在实现栈的基本功能的基础上，再实现返回栈中最小元素的操作。


### 【要求】


1、pop、push、getMin操作的时间复杂度都是O(1) 

2、设计的栈类型可以输用现成的栈结构

### 【代码实现】

```java

import java.util.Stack;

public class Mystack{

    //保存当前元素，其功能和普通栈一致
    private Stack<Integer> stackData;
    //保存最小值的栈(根据stackData栈的入栈和出栈实时变化)，栈顶为最小值
    private Stack<Integer> stackMin;


    public Mystack1() {
        //初始化双栈
        this.stackData = new Stack<>();
        this.stackMin = new Stack<>();

    }


    //stackData和stackMin入栈操作
    public void push(int newNum) {
        if (stackMin.isEmpty()) {
            stackMin.push(newNum);
        }else if (stackMin.peek()>=newNum){
            stackMin.push(newNum);
        }

        stackData.push(newNum);
    }

    //stackData和stackMin出栈操作
    public int pop(){
        if (stackData.isEmpty()){
            throw new RuntimeException("栈为空！");
        }

        int val = stackData.peek();
        if (val==stackMin.peek()){
            stackMin.pop();
        }
        stackData.pop();

        return val;
    }


    //得到当前栈的最小值
    private int getMin() {
        if (stackMin.isEmpty()) {
            throw new RuntimeException("栈为空！");
        }
        return stackMin.peek();

    }
}
```







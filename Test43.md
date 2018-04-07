## 面试题43：n个骰子的点数

题目：把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s 的所有可能的值出现的概率。


**解法一：基于递归求解，时间效率不够高。**

先把n个骰子分为两堆：第一堆只有一个，另一个有n- 1 个。单独的那一个有可能出现从1 到6 的点数。我们需要计算从1到6的每一种点数和剩下的n-1个骰子来计算点数和。接下来把剩下的n-1个骰子还是分成两堆，第一堆只有一个，第二堆有n-2个。我们把上一轮那个单独骰子的点数和这一轮单独骰子的点数相加，再和剩下的n-2个骰子来计算点数和。分析到这里，我们不难发现这是一种递归的思路，递归结束的条件就是最后只剩下一个骰子。 

我们可以定义一个长度为“6n-n+1 的数组， 和为s 的点数出现的次数保存到数组第s-n 个元素里。

实现代码：
```java
/**
 * @param number 骰子的个数
 */
public void printProbability(int number) {
    if (number < 1) {
        return;
    }

    int kind = 6;
    int minSum = number;
    int maxSum = number * 6;
    int[] probabilities = new int[maxSum - minSum + 1];
    //递归求解的过程
    probability(number, probabilities);
    //总的可能的情况
    double total = 1;
    for (int i = 0; i < number; i++) {
        total *= kind;
    }
    System.out.println("总骰子数为：" + number);
    for (int i=minSum;i<=maxSum;i++) {
        double result = probabilities[i - minSum] / total;
        System.out.printf("可能出现的点数为：%-2d,概率为：%1.4f\n", i,result);
    }
}

/**
 * 
 * @param number 骰子的个数
 * @param probabilities 筛子出现的次数（索引值+6为出现的情况）
 */
private void probability(int number, int[] probabilities) {
    for (int i=1;i<=6;i++) {
        probability(number, number, i, probabilities);
    }
}

/**
 * @param original      总的色子数
 * @param current       余要处理的色子数剩
 * @param sum           当前的骰子的点数和
 * @param probabilities 不同色子数出现次数的计数数组
 */
private void probability(int original, int current, int sum, int[] probabilities) {
    if (current == 1) {
        probabilities[sum - original]++;
    } else {
        for (int i=1;i<=6;i++) {
            probability(original, current - 1, sum + i, probabilities);
        }
    }

}

```

测试代码：
```java
@Test
public void Test() {
    printProbability(1);
    System.out.println("=====================================");
    printProbability(2);

}

```

result:
```
总骰子数为：1
可能出现的点数为：1 ,概率为：0.1667
可能出现的点数为：2 ,概率为：0.1667
可能出现的点数为：3 ,概率为：0.1667
可能出现的点数为：4 ,概率为：0.1667
可能出现的点数为：5 ,概率为：0.1667
可能出现的点数为：6 ,概率为：0.1667
=====================================
总骰子数为：2
可能出现的点数为：2 ,概率为：0.0278
可能出现的点数为：3 ,概率为：0.0556
可能出现的点数为：4 ,概率为：0.0833
可能出现的点数为：5 ,概率为：0.1111
可能出现的点数为：6 ,概率为：0.1389
可能出现的点数为：7 ,概率为：0.1667
可能出现的点数为：8 ,概率为：0.1389
可能出现的点数为：9 ,概率为：0.1111
可能出现的点数为：10,概率为：0.0833
可能出现的点数为：11,概率为：0.0556
可能出现的点数为：12,概率为：0.0278
```



**解法二：基于循环求解，时间性能好（动态规划）**
我们可以考虑用两个数组来存储骰子点数的每一个总数出现的次数。在一次循环中， 第一个数组中的第n 个数字表示骰子和为n 出现的次数。在下一循环中，我们加上一个新的骰子**，此时和为n 的骰子出现的次数应该等于上一次循环中骰子点数和为n-1 、n-2 、n-3 、n-4, n-5 与n-6 的次数的总和，所以我们把另一个数组的第n个数字设为前一个数组对应的第n-1 、n-2 、n-3 、n-4、n-5与n-6之和。**

代码实现：
```java
/**
 * @param number 骰子的个数
 */
public void printProbability(int number) {
    if (number < 1) {
        return;
    }

    //一个骰子最大的点数
    int kind = 6;
    int[][] probabilities = new int[2][number * kind + 1];


    //标记使用第一个数组还是第二个数组
    int flag = 0;

    //抛出第一个骰子可能出现的各种情况，相当于基础数据
    for (int i=1;i<=6;i++) {
        probabilities[flag][i] = 1;
    }

    // 抛出其它骰子
    for (int k = 2; k <= number; k++) {
        // 如果抛出了k个骰子，那么和为[0, k-1]的出现次数为0
        for (int i = 0; i < k; i++) {
            probabilities[1 - flag][i] = 0;
        }

        // 抛出k个骰子，所有和的可能
        for (int i = k; i <= 6 * k; i++) {
            probabilities[1 - flag][i] = 0;

            // 每个骰子的出现的所有可能的点数
            for (int j = 1; j <= i && j <= 6; j++) {
                // 统计出和为i的点数出现的次数
                probabilities[1 - flag][i] += probabilities[flag][i - j];
            }
        }

        flag = 1 - flag;
    }

    //总的可能的情况
    double total = 1;
    for (int i = 0; i < number; i++) {
        total *= kind;
    }
    System.out.println("总骰子数为：" + number);
    for (int i=number;i<=number*6;i++) {
        double result = probabilities[flag][i] / total;
        System.out.printf("可能出现的点数为：%-2d,概率为：%1.4f\n", i,result);
    }
}
```


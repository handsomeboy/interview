# 面试题67：机器人的运动范围（4.22）

标签（空格分隔）： ocoder

---

题目：地上有个m行n列的方格。一个机器人从坐标(0,0)的格子开始移动，它每一次可以向左、右、上、下移动一格，但不能进入行坐标和列坐标的数位之和大于k的格子。



思路：
1.从(0,0)开始走，每成功走一步标记当前位置为true,然后从当前位置往四个方向探索，
返回1 + 4 个方向的探索值之和。
2.探索时，判断当前节点是否可达的标准为：
1）当前节点在矩阵内；
2）当前节点未被访问过；
3）当前节点满足limit限制。


实现代码：
```java
public int movingCount(int threshold, int rows, int cols) {
    boolean[][] visited = new boolean[rows][cols];
    return countingSteps(threshold, rows, cols, 0, 0, visited);
}

public int countingSteps(int limit, int rows, int cols, int r, int c, boolean[][] visited) {
    if (r < 0 || r >= rows || c < 0 || c >= cols
            || visited[r][c] || bitSum(r) + bitSum(c) > limit) return 0;
    visited[r][c] = true;
    return countingSteps(limit, rows, cols, r - 1, c, visited)
            + countingSteps(limit, rows, cols, r, c - 1, visited)
            + countingSteps(limit, rows, cols, r + 1, c, visited)
            + countingSteps(limit, rows, cols, r, c + 1, visited)
            + 1;
}

public int bitSum(int t) {
    int count = 0;
    while (t != 0) {
        count += t % 10;
        t /= 10;
    }
    return count;
}
```





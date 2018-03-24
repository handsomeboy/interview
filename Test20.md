## 面试题20：顺时针打印矩阵


题目：输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。例如：如果输入如下矩阵：
```
1       2       3       4

5       6       7       8

9       10      11      12

13      14      15      16
```

则依次打印出数字1，2，3，4，8，12，16，15，14，13，9，5，6，7，11，10.

思路：当遇到一个复杂的问题的时候，我们可以用图形来帮助我们来思考。顺时针打印矩阵相当于外圈到内圈的顺序依次打印，我们可以把矩阵想象成若干个圈，然后确定起点，此题算法不难（基本上没有），但是当中的条件限制确是比较多。

实现代码：
```java
public ArrayList<Integer> printMatrix(int[][] matrix) {
    ArrayList<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length < 1) {
        return null;
    }

    int start = 0;
    while (matrix.length > 2 * start && matrix[0].length > 2 * start) {
        print(result, matrix, start);
        start++;
    }


    return result;
}

private void print(ArrayList<Integer> result, int[][] matrix, int start) {
    int startX = start;
    int startY = start;
    int endX = matrix.length - startX - 1;
    int endY = matrix[0].length - startY - 1;
    //从左到右打印一行
    for (int i = startY; i <= endY; i++) {
        result.add(matrix[startX][i]);
    }

    //从上到下打印一列
    if (startX < endX)
        for (int i = startX + 1; i <= endX; i++) {
            result.add(matrix[i][endY]);
        }

    //从右到左打印一行
    if (startX < endX && startY < endY)
        for (int i = endY - 1; i >= startY; i--) {
            result.add(matrix[endX][i]);
        }

    //从下到上打印一列
    if (startX < endX - 1&& startY < endY)
        for (int i = endX - 1; i > startX; i--) {
            result.add(matrix[i][startY]);
        }
}
```

测试代码：
```java
@Test
public void test() {
    int[][] numbers = {
            {1, 2, 3, 4, 5},
            {16, 17, 18, 19, 6},
            {15, 24, 25, 20, 7},
            {14, 23, 22, 21, 8},
            {13, 12, 11, 10, 9},
    };

    for (Integer integer : printMatrix(numbers)) {
        System.out.print(integer + " ");
    }

    System.out.println();

    int[][] numbers2 = {
            {1, 2, 3, 4, 5, 6, 7, 8},
            {22, 23, 24, 25, 26, 27, 28, 9},
            {21, 36, 37, 38, 39, 40, 29, 10},
            {20, 35, 34, 33, 32, 31, 30, 11},
            {19, 18, 17, 16, 15, 14, 13, 12},

    };
    for (Integer integer : printMatrix(numbers2)) {
        System.out.print(integer + " ");
    }
    System.out.println();

    int[][] numbers3 = {
            {1, 2, 3, 4, 5, 6, 7, 8}
    };
    for (Integer integer : printMatrix(numbers3)) {
        System.out.print(integer + " ");
    }
    System.out.println();

    int[][] numbers4 = {
            {1, 2, 3, 4, 5, 6, 7, 8},
            {16, 15, 14, 13, 12, 11, 10, 9}
    };
    for (Integer integer : printMatrix(numbers4)) {
        System.out.print(integer + " ");
    }
    System.out.println();


    int[][] numbers5 = {
            {1},
            {2},
            {3},
            {4},
            {5},
            {6},
            {7},
            {8}
    };
    for (Integer integer : printMatrix(numbers5)) {
        System.out.print(integer + " ");
    }
    System.out.println();

    int[][] numbers6 = {
            {0, 1},
            {15, 2},
            {14, 3},
            {13, 4},
            {12, 5},
            {11, 6},
            {10, 7},
            {9, 8}
    };
    for (Integer integer : printMatrix(numbers6)) {
        System.out.print(integer + " ");
    }
    System.out.println();


    int[][] numbers7 = {
            {1, 2},
            {4, 3}
    };
    for (Integer integer : printMatrix(numbers7)) {
        System.out.print(integer + " ");
    }
    System.out.println();
    int[][] numbers8 = {
            {1}
    };
    for (Integer integer : printMatrix(numbers8)) {
        System.out.print(integer + " ");
    }
    System.out.println();


}


```

result:
```
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 
1 2 3 4 5 6 7 8 
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 
1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 
1 2 3 4 
1 
```





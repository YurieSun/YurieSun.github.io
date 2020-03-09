---
layout: post
title: LeetCode 0054 题解
description: "螺旋矩阵"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

### 题解
1. 法一：通过`direction`数组定义右、下、左、上的顺时针方向。每次访问完计算新坐标，判断新坐标是否到达矩阵边界或者已被访问过；如果是，就需要根据顺时针方向换方向，并重新计算新坐标，反之则可以直接走到之前的新坐标。
    * 同样，这里的二维数组定义如下图所示（分别对应右、下、左、上）：
![二维数组定义]({{ '/assets/images/LeetCode/0054_1.PNG' }})
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int m = matrix.length;
        if(m == 0)
            return res;
        int n = matrix[0].length;
        boolean[][] marked = new boolean[m][n];
        int[][] direction;
        int x = 0, y = 0, k = 0;
        for(int i = 0; i < m * n; i++){
            res.add(matrix[x][y]);
            marked[x][y] = true;
            int newX = x+direction[k][0];
            int newY =y+direction[k][1];
            if(newX < 0 || newX >= m || newY < 0 || newY >= n || marked[newX][newY]){
                k = (k + 1) % 4;
                newX = x+direction[k][0];
                newY =y+direction[k][1];
            }
            x = newX;
            y = newY;
        }
        return res;
    }
}
```
2. 法二：对矩阵的每一层顺时针进行累加，示意图如下（其中`buttom`的起始位置应为`c2-1`，`left`的起始位置应为`r2`；主要关注分层方法）。这种方法可以省去标记数组，降低空间复杂度。
![逐层累加]({{ '/assets/images/LeetCode/0054_2.PNG' }})
```java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int m = matrix.length;
        if (m == 0)
            return res;
        int n = matrix[0].length;
        int r1 = 0, c1 = 0, r2 = m - 1, c2 = n - 1;
        while (r1 <= r2 && c1 <= c2) {
            for (int i = c1; i <= c2; i++)
                res.add(matrix[r1][i]);
            for (int i = r1 + 1; i <= r2; i++)
                res.add(matrix[i][c2]);
            // 只剩下一行或一列时，不需要进行下面的累加
            if (r1 < r2 && c1 < c2) {
                for (int i = c2 - 1; i >= c1 + 1; i--)
                    res.add(matrix[r2][i]);
                for (int i = r2; i > r1; i--)
                    res.add(matrix[i][c1]);
            }
            r1++;
            c1++;
            r2--;
            c2--;
        }
        return res;
    }
}
```
* 以下做法和上面的方法差不多，也是逐层累加，且每次加完一行或一列后，对边界进行增减，就像把刚才所加的行删除了一样。
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<>();
        int m = matrix.length;
        if (m == 0)
            return res;
        int n = matrix[0].length;
        int top = 0, buttom = m - 1, left = 0, right = n - 1;
        while (true) {
            for (int i = left; i <= right; i++)
                res.add(matrix[top][i]);
            if (++top > buttom)
                break;
            for (int i = top; i <= buttom; i++)
                res.add(matrix[i][right]);
            if (--right < left)
                break;
            for (int i = right; i >= left; i--)
                res.add(matrix[buttom][i]);
            if (--buttom < top)
                break;
            for (int i = buttom; i >= top; i--)
                res.add(matrix[i][left]);
            if (++left > right)
                break;
        }
        return res;
    }
}
```
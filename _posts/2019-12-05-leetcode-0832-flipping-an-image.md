---
layout: post
title: LeetCode 0832 题解
description: "翻转图像"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

### 题解
1. 法一（两次遍历）：新建矩阵，先水平翻转，再反转。
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int m = A.length, n = A[0].length;
        int[][] ans = new int[m][n];
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                ans[i][j] = A[i][n-j-1];
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++){
                if(ans[i][j] == 0)
                    ans[i][j] = 1;
                else
                    ans[i][j] = 0;
            }
        return ans;
    }
}
```
2. 法二：也可以在原始矩阵上进行操作。
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int m = A.length, n = A[0].length;
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n/2; j++){
                int temp = A[i][j];
                A[i][j] = A[i][n-j-1];
                A[i][n-j-1] = temp;
            }
                
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++){
                if(A[i][j] == 0)
                    A[i][j] = 1;
                else
                    A[i][j] = 0;
            }
        return A;
    }
}
```
3. 法三：在一次遍历中同时进行水平翻转与反转（通过异或操作进行反转，这里也可通过`1-A[i][j]`实现反转）。
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int m = A.length, n = A[0].length;
        for(int i = 0; i < m; i++)
            for(int j = 0; j < (n+1)/2; j++){
                int temp = A[i][j] ^ 1;
                A[i][j] = A[i][n-j-1] ^ 1;
                A[i][n-j-1] = temp;
            }
        return A;
    }
}
```
* 注意：此时`j`的结束索引应变为`(n+1)/2`。在两次遍历中，若列数为奇数，则中间的数不需要做交换处理；但在这里，虽然也不需要交换，但需要进行异或处理，因此改变循环结束的条件，使得中间数完成反转。
4. 法四：对矩阵的每一行，设置两个指针`left`和`right`，分别从开始与末尾进行扫描。在经过水平翻转与反转后，若对应位置的元素相等，则相当于不需要交换，只需将它们分别反转即可；若对应位置的元素不相等，则经过这两个操作后元素并未改变，因此只对相等元素进行操作。同时，当列数为奇数时，需要将中间数进行异或操作（不能将`while`循环的条件改为`left <= right`，这样使得中间数进行两次异或，相当于没有改变）。
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int m = A.length, n = A[0].length;
        for(int i = 0; i < m; i++){
            int left = 0, right = n-1;
            while(left < right){
                if(A[i][left] == A[i][right]){
                    A[i][left] ^= 1;
                    A[i][right] ^= 1;
                }
                left++;
                right--;
            }
            if(left == right)
                A[i][left] ^= 1;
        }
        return A;
    }
}
```
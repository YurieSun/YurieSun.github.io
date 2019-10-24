---
layout: post
title: LeetCode 0070 题解
description: "爬楼梯"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)

### 思路
发现是一个斐波那契数列的问题，因此可采用递归或动态规划。

### 题解
* 法一：递归（会超时）
```java
class Solution {
    public int climbStairs(int n) {
        if(n == 1) return 1;
        if(n == 2) return 2;
        return climbStairs(n-1) + climbStairs(n-2);
    }
}
```

* 法二：将数列每一项记录到数组中，相当于用空间换时间（也是动态规划的解法）
```java
class Solution {
    public int climbStairs(int n) {
        int[] a = new int[n+1];
        a[0] =1;
        a[1] =1;
        for (int i = 2; i <= n; i++)
            a[i] = a[i-2] +a[i-1];
        return a[n];
    }
}
```  
这里要注意：是用数组的第`n`项还是第`n-1`项表示爬$n$阶楼梯的方法，这会影响数组大小、数组前两项值的选择、是否有特殊情况需要提前返回、$for$循环中`i`的终止值等。

* 法三：和上面方法类似，只是不用数组储存（因此节省了空间）
```java
class Solution {
    public int climbStairs(int n) {
        if(n == 1) return 1;
        int first = 1, second = 2;
        for(int i = 3; i <= n; i++){
            int sum = first + second;
            first = second;
            second = sum;
        }
        return second;
    }
}
```

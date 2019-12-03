---
layout: post
title: LeetCode 0746 题解
description: "使用最小花费爬楼梯"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

### 题解
1. 法一（动态规划）：设爬一个阶梯到达当前阶梯的花费为`f1`，爬两个阶梯到达当前阶梯的花费为`f2`，由于初始阶梯无花费，因此把它们均初始化为0。对于遍历的每一个阶梯，爬到此阶梯的最小花费为【之前爬两个阶梯到此阶梯】和【之前爬一个阶梯到此阶梯】的较小值加上当前阶梯花费。同时，当前的`f1`相当于下一阶梯的`f2`，当前的花费`cur`相当于下一阶梯的`f1`，因此在循环中不断更新`f1`和`f2`。遍历结束后，`f1`和`f2`存储的分别是爬一个阶梯和爬两个阶梯到达阶梯顶（即数组最后一个元素的后面一个位置）的花费，因此，返回其中的较小值即可。
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int f1 = 0, f2 = 0;
        for(int i = 0; i < cost.length; i++){
            int cur = Math.min(f1, f2) + cost[i];
            f2 = f1;
            f1 = cur;
        }
        return Math.min(f1, f2);
    }
}
```
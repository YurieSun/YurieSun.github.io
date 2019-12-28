---
layout: post
title: LeetCode 0123 题解
description: "买卖股票的最佳时机 III"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

### 题解
1. 法一（动态规划）：写出状态方程如下（其中`i`表示第几天，`k`表示剩余交易次数，`0`表示不持有股票，`1`表示持有股票）
    ```
    dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
    dp[i][k][1] = Math.max(dp[i-1][k-1][0] - prices[i], dp[i-1][k][1]);
    ```  
    根据状态方程，可写出关于`i`和`k`的两重循环。同时要注意初始值的选取如下
    ```
    [-1][k][0] = 0;
    [-1][k][1] = Integer.MIN_VALUE;
    [i][0][0] = 0;
    [i][0][1] = Integer.MIN_VALUE;
    ```  
    由此可得以下算法：
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0)
            return 0;
        int max_k = 2;
        int[][][] dp = new int[prices.length][max_k + 1][2];
        for(int i = 0; i < prices.length; i++){
            for(int k = max_k; k >= 1; k--){
                if(i - 1 == -1){
                    dp[i][k][0] = 0;
                    dp[i][k][1] = -prices[i];
                    continue;
                }
                dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
                dp[i][k][1] = Math.max(dp[i-1][k-1][0] - prices[i], dp[i-1][k][1]);
            }
        }  
        return dp[prices.length - 1][max_k][0];
    }
}
```
* 由于三维数组中每个元素的计算只和前面一次的计算结果有关，因此采用变量进行保存，使空间复杂度降为$O(1)$。
```java
class Solution {
    public int maxProfit(int[] prices) {
        int dp_i20 = 0, dp_i21 = Integer.MIN_VALUE;
        int dp_i10 = 0, dp_i11 = Integer.MIN_VALUE;
        for(int n : prices){
            dp_i20 = Math.max(dp_i20, dp_i21 + n);
            dp_i21 = Math.max(dp_i10 - n, dp_i21);
            dp_i10 = Math.max(dp_i10, dp_i11 + n);
            dp_i11 = Math.max(-n, dp_i11);
        }
        return dp_i20; 
    }
}
```
* 股票系列的动态规划详细解法参考[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)。
* 英文版的股票系列详解[Most consistent ways of dealing with the series of stock problems](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/discuss/108870/Most-consistent-ways-of-dealing-with-the-series-of-stock-problems)。
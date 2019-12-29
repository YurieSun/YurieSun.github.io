---
layout: post
title: LeetCode 0714 题解
description: "买卖股票的最佳时机含手续费"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

### 题解
1. 法一（动态规划）：写出状态方程如下（其中`i`表示第几天，`0`表示不持有股票，`1`表示持有股票），并加入手续费的影响。买入时减去手续费的方程如下：
    ```
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
    dp[i][1] = Math.max(dp[i-1][0] - prices[i] - fee, dp[i-1][1]);
    ```  
    卖出时减去手续费的方程如下：  
    ```
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i] - fee);
    dp[i][1] = Math.max(dp[i-1][0] - prices[i], dp[i-1][1]);
    ```
    初始值的选取如下:
    ```
    [-1][0] = 0;
    [-1][1] = Integer.MIN_VALUE;
    ```  
    直接使用压缩空间的算法（第一种状态方程）：
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int dp_i0 = 0, dp_i1 = Integer.MIN_VALUE;
        for(int n : prices){
            int dp_pre0 = dp_i0;
            dp_i0 = Math.max(dp_i0, dp_i1 + n);
            dp_i1 = Math.max(dp_pre0 - n - fee, dp_i1);
        }
        return dp_i0;
    }
}
```
* 第二种状态方程（这里要注意溢出问题，由于`dp_i1`初始化为最小的负数，在后面减去`fee`可能会导致超出最小负数值，从而溢出；而第一种情况则不会，因为与`dp_i1`相关的是加法，而`dp_pre0`与减法相关，其初始值为`0`，因此不会溢出）：
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        long dp_i0 = 0, dp_i1 = Integer.MIN_VALUE;
        for(int n : prices){
            long dp_pre0 = dp_i0;
            dp_i0 = Math.max(dp_i0, dp_i1 + n - fee);
            dp_i1 = Math.max(dp_pre0 - n, dp_i1);
        }
        return (int)dp_i0;
    }
}
```

* 股票系列的动态规划详细解法参考[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)。
* 英文版的股票系列详解[Most consistent ways of dealing with the series of stock problems](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/discuss/108870/Most-consistent-ways-of-dealing-with-the-series-of-stock-problems)。
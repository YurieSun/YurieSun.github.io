---
layout: post
title: LeetCode 0309 题解
description: "最佳买卖股票时机含冷冻期"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

### 题解
1. 法一（动态规划）：写出状态方程如下（其中`i`表示第几天，`0`表示不持有股票，`1`表示持有股票）。由于具有冷冻期，则第二个状态方程有所变化，出现了`i-2`天的情况。这里需要注意的是，`dp[i][1]`表示第`i`天买入，`dp[i-2][0]`表示第`i-2`天未持有股票，则有两种情况：(1)第`i-2`天卖出，则需要在间隔一天后在第`i`天买入，此时第二个状态方程右侧第一项应该是`dp[i-2][0] - prices[i]`；(2)第`i-2`天未卖出，第`i-1`天也卖出时，第`i`天也可以进行买入，此时第二个状态方程右侧第一项应该是`dp[i-1][0] - prices[i]`，而这两天没有交易的情况下，有`dp[i-1][0]=dp[i-2][0]`。因此`dp[i-2][0] - prices[i]`是可以概括上述两种情况的。
    ```
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
    dp[i][1] = Math.max(dp[i-2][0] - prices[i], dp[i-1][1]);
    ```  
    初始值的选取如下:
    ```
    [-2][0] = 0;
    [-1][0] = 0;
    [-1][1] = Integer.MIN_VALUE;
    ```  
    由此可得以下算法（由于数组中[-2]不好表示，因此直接采用压缩空间的方法）：
```java
class Solution {
    public int maxProfit(int[] prices) {
        int dp_i0 = 0, dp_i1 = Integer.MIN_VALUE;
        int dp_pre0 = 0;
        for(int n : prices){
            //这里通过temp和保存dp_pre0上上次未持有的状态
            int temp = dp_i0;
            dp_i0 = Math.max(dp_i0, dp_i1 + n);
            dp_i1 = Math.max(dp_pre0 - n, dp_i1);
            dp_pre0 = temp;
        }
        return dp_i0;
    }
}
```
* 股票系列的动态规划详细解法参考[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)。
* 英文版的股票系列详解[Most consistent ways of dealing with the series of stock problems](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/discuss/108870/Most-consistent-ways-of-dealing-with-the-series-of-stock-problems)。
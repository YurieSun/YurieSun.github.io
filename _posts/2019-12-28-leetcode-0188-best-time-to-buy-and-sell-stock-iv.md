---
layout: post
title: LeetCode 0188 题解
description: "买卖股票的最佳时机 IV"
keywords: test
category: LeetCode
tags: [solving LeetCode]
---

### 题目描述
[买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

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
    public int maxProfit(int k, int[] prices) {
        if(prices.length == 0)
            return 0;
        int n = prices.length;
        //最大交易次数为数组长度的一半，因此超过该数的k没有限制作用，相当于不限制交易次数的情况。
        //当k太大时，三维数组占用空间很大，会出现超出内存限制的错误，所以对于很大的k，采用买卖股票的最佳时机 II中的贪心算法。
        if(k > n/2)
            return maxProfit_2(prices);
        int[][][] dp = new int[n][k+1][2];
        for(int i = 0; i < n; i++){
            for(int j = k; j >= 1; j--){
                if(i - 1 == -1){
                    dp[i][j][0] = 0;
                    dp[i][j][1] = -prices[i];
                    continue;
                }
                dp[i][j][0] = Math.max(dp[i-1][j][0], dp[i-1][j][1] + prices[i]);
                dp[i][j][1] = Math.max(dp[i-1][j-1][0] - prices[i], dp[i-1][j][1]); 
            }
        }
        return dp[n-1][k][0];
    }
    public int maxProfit_2(int[] prices){
        int profit = 0;
        for(int i = 1; i < prices.length; i++)
            if(prices[i-1] < prices[i])
                profit += prices[i] - prices[i-1];
        return profit;
    }
}
```
* 本题中由于`k`的大小未知且有可能很大，因此无法用变量列举的方法来压缩空间复杂度，只能采用三维数组。但可以发现每一天的状态只和前一天有关，因此可以压缩`i`这个维度，采用二维数组。（这其实和之前的变量列举是一样的，都是通过只记录前一天的状态来压缩天数`i`这一维度。）
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if(prices.length == 0)
            return 0;
        int n = prices.length;
        if(k > n/2)
            return maxProfit_2(prices);
        int[][] dp = new int[k+1][2];
        for(int i = 0; i < n; i++){
            for(int j = k; j >= 1; j--){
                if(i - 1 == -1){
                    dp[j][0] = 0;
                    dp[j][1] = -prices[i];
                    continue;
                }
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                dp[j][1] = Math.max(dp[j-1][0] - prices[i], dp[j][1]); 
            }
        }
        return dp[k][0];
    }
    public int maxProfit_2(int[] prices){
        int profit = 0;
        for(int i = 1; i < prices.length; i++)
            if(prices[i-1] < prices[i])
                profit += prices[i] - prices[i-1];
        return profit;
    }
}
```
* 股票系列的动态规划详细解法参考[一个通用方法团灭 6 道股票问题](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)。